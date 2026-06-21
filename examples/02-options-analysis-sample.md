# Sample Output: Options Analysis

This is a worked example of the [Options Analysis](../prompts/02-options-analysis.md) prompt, continuing the scenario from the [Requirements Capture sample](./01-requirements-capture-sample.md).

## Sample input

Requirements: the output from the Requirements Capture sample.

Constraints: 2 engineers, one quarter timeline. Three existing systems: an on-prem OMS (REST API), a SaaS ecommerce platform (OAuth API), and a third-party fulfillment partner (API key, documented p95 latency of ~2s, undocumented rate limits).

## Sample output

### Option 1: Real-Time Aggregation Service
A stateless API that queries all three systems live on each request and returns a normalized, combined view.

- **Approach:** Agent UI calls a single new endpoint. The aggregator fans out in parallel to the OMS, ecommerce platform, and fulfillment partner via per-system adapter clients, normalizes the results into a common schema, tags each field with its source system, and returns the merged response. A short-TTL in-memory cache absorbs repeat lookups within the same call.
- **Pros/cons vs. NFRs:**
  - *Consistency:* Meets it fully — always reflects current upstream state, no staleness window.
  - *Performance:* Meets it only if all three upstreams respond inside the load-time budget; the fulfillment partner's ~2s p95 is the risk.
  - *Security:* Meets it well — only the aggregator holds upstream credentials, and all lookups can be logged centrally for audit.
  - *Scalability:* Partially meets it — the service itself scales horizontally, but peak agent traffic multiplies load on upstream systems, especially the rate-limited partner API.
- **Cost/effort:** Medium. Driver: building and maintaining three different auth/protocol adapters.
- **Risks:** Fulfillment partner latency or undocumented rate limits could blow the load-time budget or get the aggregator throttled.

### Option 2: Nightly Batch Warehouse
A nightly ETL job lands data from all three systems into one read-optimized store; the UI reads only from that store.

- **Approach:** Scheduled jobs pull from each system once per night, normalize, and write into a shared table. Agent UI queries this table directly — no live dependency on the three systems at read time.
- **Pros/cons vs. NFRs:**
  - *Performance:* Meets it easily — local reads are fast and don't depend on upstream latency.
  - *Scalability:* Meets it well — read load is fully decoupled from the three source systems.
  - *Consistency:* Does not meet it — data can be up to 24h stale, which directly conflicts with the "agents shouldn't work from stale data mid-call" requirement.
  - *Security:* Partially meets it — introduces a new data store holding PII that now needs its own access controls and retention policy.
- **Cost/effort:** Medium-low. Driver: building ETL pipelines is less integration-heavy than three sets of live-call adapters, but adds ongoing pipeline maintenance.
- **Risks:** Silent ETL failures produce stale or missing data with no obvious signal to the agent unless a "last updated" indicator is built and agents are trained to check it.

### Option 3 (rejected alternative): Embedded System Views
Wrap the three existing system screens side by side in one page, with no real backend aggregation.

- **Approach:** A composite UI embeds each system's existing screen/widget for the same customer, using existing SSO across the three portals.
- **Pros/cons vs. NFRs:** Cheapest to ship, but does not meet the core goal — agents still mentally cross-reference three separate UIs, so it doesn't reduce errors or produce "one unified view," and there's no way to satisfy "tag each order line with its source system" since there's no unified data model at all.
- **Cost/effort:** Low. Driver: no backend integration, just UI composition.
- **Risks:** Ships fast but likely fails to move the metrics (handle time, error rate) the brief actually cares about — included here mainly as the obvious-but-wrong baseline.

### Comparison Table

| Option | Performance | Security | Scalability | Consistency |
|---|---|---|---|---|
| 1. Real-Time Aggregation | Partially meets | Meets | Partially meets | Meets |
| 2. Nightly Batch Warehouse | Meets | Partially meets | Meets | Does not meet |
| 3. Embedded Views | Meets | Meets | Meets | Does not meet (no aggregation) |

### Recommendation
**Option 1 — Real-Time Aggregation Service.** It's the only option that satisfies the freshness requirement without abandoning the "single unified view" goal from the brief. The performance risk (fulfillment partner latency) is real but addressable with per-system timeouts and a partial-result UI, whereas Option 2's staleness is a fundamental mismatch with how the tool will be used (live, on-call).

### When to choose differently
If the fulfillment partner can't reliably meet the load-time budget and won't commit to a latency SLA, fall back to a hybrid: live calls for the two fast systems, nightly batch (with a visible "last synced" timestamp) for the fulfillment partner only.
