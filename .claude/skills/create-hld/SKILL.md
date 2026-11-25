---
name: create-hld
description: Write a high-level solution design document (HLD) and save it to docs/hld/. Use when the user asks to "create an HLD", "write a high-level design", "write a solution design doc", or wants a design doc once an ADR has been accepted.
---

# Create HLD

Write a high-level solution design document that engineers and stakeholders can review and build from, and save it as a markdown file.

## 1. Load the template

Read `prompts/04-solution-design-doc.md`. The prompt block and its Tips set the required sections, their order, and the rules for each. Read `examples/04-solution-design-doc-sample.md` to see the expected shape and level of detail. Follow the prompt file, not memory, since it may have changed.

## 2. Gather the inputs

You need:

- **Requirements:** functional and non-functional.
- **Accepted ADR(s):** if the user doesn't name any, list `docs/adr/` and offer the relevant ones.
- **Existing systems and integration points.**
- **Target audience:** for example engineers, non-technical stakeholders, or both. If this isn't given, ask, because it changes how the whole doc is written.

Take these from the skill's arguments, from the conversation, or from files the user points to. If the user is working in a codebase, read the relevant code or config to ground the System Context and Architecture sections. For other gaps, ask a short question if the gap is significant. Otherwise, continue and flag it.

## 3. Write the HLD

- Follow the sections in the prompt file in order.
- In **System Context**, keep the precise written description the prompt asks for, and add a Mermaid `flowchart` under it. Follow the diagram conventions in `.claude/skills/arch-analysis/SKILL.md`: label every edge with what flows across it, put a `subgraph` around the system's own components, and keep it to roughly 15 nodes.
- In **Non-Functional Considerations**, refer to each non-functional requirement by name or number and say how the design meets it.
- Mark anything that isn't backed by the inputs as `**Assumption:**` inline, rather than filling the gap silently.
- Write the **Overview** so it makes sense on its own, since some readers stop there.

## 4. Save it

Create `docs/hld/` if it doesn't exist, and save the doc as `docs/hld/<kebab-case-system-name>.md`. If that file already exists, ask whether to overwrite it or save under a new name.

## 5. Report back

In chat, give the file path, a 2–3 sentence summary of the design, the assumptions you flagged, and the open risks. Don't print the whole doc unless asked. Suggest `/review-requirements` or `/review-standards` as a next step.
