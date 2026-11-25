---
name: create-adr
description: Write an Architecture Decision Record (ADR) and save it to docs/adr/ with the next ADR number. Use when the user asks to "create an ADR", "write an ADR", "record this decision", or has picked an option and wants it documented.
---

# Create ADR

Write a new ADR for a decision that has already been made, and save it as a numbered markdown file.

## 1. Load the template

Read `prompts/03-architecture-decision-record.md`. The prompt block and its Tips set the required sections, their order, and the rules for each. Read `examples/03-architecture-decision-record-sample.md` to see the expected tone, length, and level of detail. Follow the prompt file, not memory, since it may have changed.

## 2. Gather the inputs

You need:

- **Requirements:** the problem and constraints driving the decision.
- **Options considered:** ideally with a comparison, such as the output of `prompts/02-options-analysis.md`.
- **Chosen option:** what was decided.
- **Reasoning:** why it was chosen. This is optional, since it can be inferred from the comparison.

Take these from the skill's arguments, from the conversation, or from files the user points to. If the **chosen option** isn't clear, ask the user for it; never pick one yourself. For other gaps, ask a short question if the gap would change the decision. Otherwise, continue and flag it as an assumption.

## 3. Write the ADR

- Follow the sections in the prompt file in order.
- Use the title format from the prompt: describe the decision, not the problem.
- Set **Status** to `Proposed` unless the user says the decision has been accepted.
- Add today's date under the title.
- Make **Consequences** specific to this decision: what gets easier, what gets harder, and what new risks or work it creates. Don't let it go generic.
- Mark anything that isn't backed by the inputs as `**Assumption:**` inline, rather than filling the gap silently.

## 4. Save it

1. List `docs/adr/`, creating it if it doesn't exist.
2. Find the highest existing `ADR-NNN` number and add one. If there are none, start at `001`. Always pad to 3 digits.
3. Save the ADR as `docs/adr/ADR-NNN-<kebab-case-title>.md`, keeping the slug to about 6 words. Use the same number in the heading, e.g. `# ADR-007: Use event-driven sync instead of nightly batch`.

## 5. Report back

In chat, give the file path, the decision in one sentence, any assumptions you flagged, and the follow-ups. Don't print the whole ADR unless asked. Suggest `/review-requirements` or `/review-standards` as a next step.
