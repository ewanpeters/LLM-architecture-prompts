# Prompt: Architecture Decision Record (ADR)

**Stage 3 of 4** — turns a chosen option into a formal, durable decision record.

**Use when:** you've picked an option from [Options Analysis](./02-options-analysis.md) and need to record the decision for future readers, including future you.

## Prompt

```
You are a senior solution architect writing an Architecture Decision Record (ADR). Use the standard ADR format (Context, Decision, Options Considered, Consequences).

Inputs:
- Requirements: [PASTE OR SUMMARIZE]
- Options considered and the comparison table: [PASTE FROM OPTIONS ANALYSIS]
- Chosen option: [NAME THE OPTION]
- Reasoning for the choice, if you already have it: [OPTIONAL - describe, or leave blank and ask the model to infer it from the comparison table]

Write the ADR with these sections:
1. Title - short and specific, describes the decision, not the problem (e.g. "Use event-driven sync instead of nightly batch for inventory updates").
2. Status - Proposed / Accepted / Superseded.
3. Context - the forces at play: requirements, constraints, why a decision is needed now.
4. Decision - what was decided, stated as a single clear sentence, followed by supporting detail.
5. Options Considered - brief recap of each option from the analysis and why it was or wasn't chosen.
6. Consequences - what becomes easier, what becomes harder, what new risks or follow-up work this creates.
7. Follow-ups - concrete next actions and what/who they're blocked on.

Keep it concise enough that someone unfamiliar with the project can read it in five minutes and understand both the decision and why it was made.
```

## Tips

- One ADR per decision — if you're recording three decisions, write three ADRs.
- Number and date ADRs (e.g. `ADR-007-event-driven-inventory-sync.md`) so history stays legible as decisions get superseded.
- The "Consequences" section is the one people skip writing and the one that matters most in a year — don't let the model leave it generic.
- See a [sample output](../examples/03-architecture-decision-record-sample.md) for what to expect.
