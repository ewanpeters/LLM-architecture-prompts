# Sample Output: Architecture Decision Record

This is a worked example of the [ADR](../prompts/03-architecture-decision-record.md) prompt, continuing the scenario from the [Options Analysis sample](./02-options-analysis-sample.md).

## Sample input

Chosen option: Option 1, Real-Time Aggregation Service, from the Options Analysis sample.

## Sample output

### ADR-001: Use a real-time aggregation service instead of nightly batch sync for the unified order view

**Status:** Accepted

**Context:**
Support agents currently check three separate systems (an on-prem OMS, a SaaS ecommerce platform, and a third-party fulfillment partner) to assemble a customer's order history during live calls. This is slow and error-prone. We need a single view that agents can trust to be current, since they're using it while a customer is on the phone — stale data here directly produces wrong answers to customers, not just inconvenience.

**Decision:**
We will build a stateless aggregation API that queries the OMS, ecommerce platform, and fulfillment partner live, in parallel, on each request, and returns a normalized, source-tagged result to the agent UI. Each upstream call has its own timeout; the service returns partial results rather than failing outright if one system is slow or unavailable. Results are cached in-memory for a short TTL to absorb repeat lookups within a single call.

**Options Considered:**
- *Real-time aggregation (chosen):* Meets the freshness requirement; main risk is the fulfillment partner's latency and rate limits.
- *Nightly batch warehouse (rejected):* Would have met performance and scalability cleanly, but up to 24h of staleness is incompatible with agents using this data live, on calls.
- *Embedded system views (rejected):* Cheapest, but doesn't actually unify the data — agents would still be manually cross-referencing, which fails the brief's core goal.

**Consequences:**
- *Easier:* Agents always see current data; no second data store to keep in sync or reconcile; all lookups can be audited centrally at the aggregator.
- *Harder:* The aggregator's availability is now coupled to the availability of all three upstream systems; we need real timeout/circuit-breaker behavior rather than assuming upstreams are always healthy.
- *New risks:* The fulfillment partner's ~2s p95 latency and undocumented rate limits become a shared resource the aggregator must protect against (backoff, per-system timeout, partial results) — this wasn't a concern under the old per-system manual lookup process, since agents naturally throttled themselves.

**Follow-ups:**
- Confirm a load-time SLA with support ops (currently an assumption carried from Requirements Capture).
- Get the fulfillment partner team to confirm or set explicit rate limits.
- Design and build the partial-result UI state (what an agent sees when one system times out).
- Pick and tune the cache TTL value.
