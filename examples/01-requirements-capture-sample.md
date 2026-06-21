# Sample Output: Requirements Capture

This is a worked example of the [Requirements Capture](../prompts/01-requirements-capture.md) prompt, showing the shape of output you should expect.

## Sample input brief

```
Customer support agents currently have to check three different systems to see a customer's
order history when they call in. We want a unified view so agents can see everything in one
place. This should save time on calls and reduce errors.
```

## Sample output

### Problem Summary
Customer support agents currently must manually cross-reference three separate systems to assemble a full picture of a customer's order history during live calls, which slows down calls and increases the risk of agents missing or misreporting information. The proposed solution is a unified view that consolidates order history from these systems into a single screen, aiming to reduce average handle time and reduce errors during support interactions.

### Functional Requirements
1. Display a customer's order history (order ID, date, status, items, value) from all three source systems in a single view, searchable by customer identifier (account number, email, or phone).
2. Reflect changes from any source system within [X seconds/minutes] of the change, so agents aren't working from stale data mid-call.
3. Return search results and open a customer's full order history in under [X seconds] from query submission.
4. Indicate, per order line, which source system the data originated from, so agents can escalate discrepancies to the right team.
5. Restrict access to authenticated support agents with an active session, consistent with existing support-tooling access controls.

### Non-Functional Requirements
- **Performance:** Order history must load within [X seconds] of search submission — no target given in the brief.
- **Availability:** Must be available during all support operating hours.
- **Security:** Order data is PII; access must be restricted and lookups auditable.
- **Scalability:** Must support concurrent use by all active agents during peak call volume.
- **Consistency:** Must tolerate the three source systems being eventually (not instantly) consistent with each other.

### Assumptions
- A load time of a few seconds is needed for live-call usability — the brief says "save time on calls" but gives no number.
- "Support agents" is an existing, already-authenticated user group (existing SSO), not a new user base to provision.
- The three source systems expose some queryable interface (API/DB/export) rather than requiring screen-scraping.
- Read-only access is sufficient — the brief doesn't ask for editing order data from the new view.

### Open Questions
1. What are the three source systems, and what integration options do they expose?
2. Is "reduce errors" about agents misquoting order info, miskeying lookups, or something else?
3. Is there a required SLA for data freshness?
4. Should the view show order history only, or also adjacent data agents currently check (shipping, refunds, support tickets)?
5. Who owns each source system, and are any of them slated for change/deprecation?

### Out of Scope
- Editing/correcting order data from the new view.
- Customer-facing access (internal agent tool only).
- Migrating or decommissioning any source system.
- Reporting/analytics beyond a single-customer lookup during a call.
