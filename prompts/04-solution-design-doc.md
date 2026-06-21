# Prompt: High-Level Solution Design Doc

**Stage 4 of 4** — produces the full solution architecture doc once the decision is made.

**Use when:** the ADR is accepted and you need a doc that engineers, reviewers, and stakeholders can actually build from.

## Prompt

```
You are a senior solution architect writing a high-level solution design document, to be reviewed by engineers and technical stakeholders before implementation begins.

Inputs:
- Requirements: [PASTE OR LINK]
- Accepted ADR(s): [PASTE OR LINK]
- Existing systems / integration points: [LIST]
- Target audience for this doc: [E.G. "engineers who'll build it" / "non-technical stakeholders" / "both"]

Produce a document with these sections:
1. Overview - what is being built and why, in plain language, 2-3 sentences.
2. System Context - what this system talks to: actors, upstream/downstream systems, external dependencies. Describe it precisely enough that someone could draw the diagram from your text.
3. Architecture - key components and their responsibilities, how they communicate (sync/async, protocols), and where state lives.
4. Data - what data is created/read/updated, where it's stored, and any notable lifecycle/retention concerns.
5. Non-Functional Considerations - how the design satisfies each non-functional requirement from the requirements doc (be specific, reference the actual requirement).
6. Failure Modes - what happens when each major component or dependency fails, and how the system degrades or recovers.
7. Rollout Plan - phases, feature flags, migration steps, and rollback plan.
8. Open Risks - anything unresolved that the reviewer should weigh in on.

Flag any place where you had to assume something not in the inputs, instead of silently filling the gap.
```

## Tips

- Ask for "describe it precisely enough to draw" rather than asking for a diagram — models fake diagrams badly, but they're good at descriptions a human (or a diagramming tool) can turn into one.
- Do a second pass where you ask the model to check its own doc against the original requirements line-by-line and flag anything it didn't address.
- This doc is the one stakeholders will actually read in full — keep the Overview good enough to stand alone.
- See a [sample output](../examples/04-solution-design-doc-sample.md) for what to expect.
