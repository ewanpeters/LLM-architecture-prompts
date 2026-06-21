# Sample Output: Solution Design Doc

This is a worked example of the [Solution Design Doc](../prompts/04-solution-design-doc.md) prompt, continuing the scenario from the [ADR sample](./03-architecture-decision-record-sample.md).

## Sample output

### 1. Overview
We're building a unified order-history view for customer support agents, backed by a new real-time aggregation service that queries three existing systems live. This replaces the current process where agents manually check each system during a call, with the goal of reducing handle time and lookup errors.

### 2. System Context
**Actors:** Support agent (user, via existing agent desktop UI).

**New component:** Aggregation Service.

**Upstream systems:** On-prem OMS (REST, service-account auth), SaaS ecommerce platform (OAuth), third-party fulfillment partner (API key, IP allowlisted, ~2s p95 latency).

**Flow:** Agent UI sends a single request to the Aggregation Service for a given customer → the service calls all three upstream systems in parallel, each through its own adapter → results are normalized into a common order schema, tagged by source system → merged response returned to the Agent UI for display.

### 3. Architecture
- **Agent UI:** Existing support console, extended with a new "Order History" panel that calls the Aggregation Service.
- **Aggregation Service:** New, stateless, horizontally scalable. Exposes `GET /customers/{id}/orders`. Fans out to three upstream adapters in parallel, each with its own timeout (e.g. 1.5s). Normalizes results into a shared `Order` schema, tagging each field with its source system. Holds a short-TTL (~30s) in-memory cache to absorb repeat lookups within a single call.
- **Upstream adapters:** One per source system, isolating auth and protocol differences — OMS service-account REST, ecommerce OAuth, fulfillment partner API key.

**Communication:** Synchronous request/response, Agent UI → Aggregation Service → upstream systems (parallel, with timeouts). No persistent state beyond the ephemeral cache.

**Where state lives:** No new system of record. The Aggregation Service is stateless except for its short-lived cache.

### 4. Data
- **Created:** None — the service is read-only.
- **Read:** Order records from the three upstream systems.
- **Stored:** Nothing persisted beyond the ephemeral in-memory cache (~30s TTL, not written to disk).
- **Retention:** Audit log of lookups (agent ID, customer ID, timestamp) retained per existing compliance policy. No PII persisted by the new service itself.

### 5. Non-Functional Considerations
- **Performance:** Met via parallel fan-out with per-system timeouts; partial results are shown rather than blocking on the slowest system (see Failure Modes).
- **Availability:** Stateless service, horizontally scaled behind a load balancer; availability is still bounded by the weakest upstream dependency, mitigated with timeouts and circuit breakers rather than full availability guarantees.
- **Security:** All upstream credentials live only in the Aggregation Service, not the UI. Every lookup is logged with agent ID, customer ID, and timestamp. Access requires the agent's existing SSO session, validated by the service.
- **Scalability:** The service scales horizontally; peak concurrent load is bounded by the fulfillment partner's rate limits, which may require request coalescing or queuing under peak load.
- **Consistency:** Always reflects live upstream state at call time — fresher than a batch approach, but only as fast as the slowest successful upstream call.

### 6. Failure Modes
- **One upstream times out or errors:** Return a partial result; clearly label the affected system as "unavailable" rather than failing the whole request.
- **All three upstreams unavailable:** Surface a clear error with retry guidance to the agent; log the incident.
- **Fulfillment partner rate-limits the service:** Back off, trip a circuit breaker, serve cached/partial data, alert on-call.
- **Cache layer failure:** Degrade to direct upstream calls — the cache is a latency optimization, not a dependency.

### 7. Rollout Plan
- **Phase 1:** Internal beta with a small group of agents, behind a feature flag, covering only the OMS and ecommerce platform (2 of 3 systems) to validate latency and UX.
- **Phase 2:** Add the fulfillment partner integration once rate limits are confirmed.
- **Phase 3:** Roll out to all agents; retire the old per-system manual lookup documentation.
- **Rollback:** Flip the feature flag off to revert agents to the existing per-system workflow. No data migration to undo, since the service is read-only and stateless.

### 8. Open Risks
- The fulfillment partner's rate limits and latency SLA aren't contractually confirmed yet — could force a hybrid design (batch caching for that one system) if they can't be met.
- No confirmed load-time SLA from support ops; the current target is an assumption carried from Requirements Capture.
- Audit log retention requirements aren't yet confirmed with compliance.

**Assumptions flagged:** The load-time target, the fulfillment partner's API stability, and the sufficiency of read-only access are all carried forward as unconfirmed assumptions from earlier stages and should be validated before this design is finalized.
