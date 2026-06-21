# Prompt: Requirements Capture

**Stage 1 of 4** — turns a rough brief into structured, reviewable requirements.

**Use when:** you have a rough product ask (a ticket, a paragraph from a stakeholder, a Slack thread) and need to turn it into requirements you can actually design against.

## Prompt

```
You are a senior solution architect helping me turn a rough product brief into a structured requirements document.

Here is the brief:
"""
[PASTE RAW BRIEF / TICKET / NOTES HERE]
"""

Additional context (delete if not applicable):
- Existing systems this touches: [LIST]
- Known constraints (budget, timeline, team, compliance): [LIST]
- Stakeholders and their priorities: [LIST]

Do the following:
1. Restate the problem in one paragraph, in your own words, so I can confirm you understood it correctly.
2. Extract functional requirements as a numbered list. Each one should be testable — avoid vague verbs like "support" or "handle" without specifying what success looks like.
3. Extract non-functional requirements (performance, availability, security, compliance, cost, scalability). Call out any that are implied but not stated, and flag those as assumptions.
4. List open questions — anything ambiguous in the brief that needs a stakeholder answer before design can start.
5. List out-of-scope items — things adjacent to this brief that you are explicitly NOT including, to prevent scope creep.

Output as markdown with headings: Problem Summary, Functional Requirements, Non-Functional Requirements, Assumptions, Open Questions, Out of Scope.
```

## Tips

- Paste the real text, not a summary of it — the model catches ambiguity better from raw language than from your paraphrase.
- Push back on the functional requirements until each line is independently testable.
- Send the **Open Questions** section to stakeholders before moving on to [Options Analysis](./02-options-analysis.md) — cheaper to resolve ambiguity now than after you've designed around a wrong assumption.
- See a [sample output](../examples/01-requirements-capture-sample.md) for what to expect.
