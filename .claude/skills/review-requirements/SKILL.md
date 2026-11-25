---
name: review-requirements
description: Check an ADR or HLD against a requirements document, requirement by requirement, and report gaps, conflicts and scope creep. Use when the user asks to "check this ADR/HLD against the requirements", "does this design meet the requirements", or wants a requirements traceability review.
---

# Review Against Requirements

Check whether an ADR or HLD actually meets its requirements, one requirement at a time. This is a review only: don't edit the document being reviewed.

## 1. Gather the inputs

You need:

- **The document to review:** an ADR or HLD, such as a file in `docs/adr/` or `docs/hld/`.
- **The requirements:** a file path or pasted text, ideally in the format from `prompts/01-requirements-capture.md`.

If either is missing, ask for it. Don't review against requirements you've had to guess.

## 2. Check each requirement

List every functional and non-functional requirement, keeping the requirements doc's numbering. If the requirements aren't numbered, number them as `FR1`, `FR2`… and `NFR1`, `NFR2`….

Give each requirement one of these ratings:

| Rating | Meaning |
|---|---|
| **Addressed** | The doc clearly meets it. |
| **Partial** | The doc covers it in part, or only vaguely, e.g. "will be fast" against "under 2 seconds". |
| **Not addressed** | The doc doesn't mention it. |
| **Conflicts** | The doc's design contradicts it. |

Cite the section of the doc each rating relies on. Be strict: a requirement that is only restated, with no explanation of how it's met, is **Partial**. If a requirement still contains an unresolved placeholder, such as `[X seconds]`, call that out, because it can't be properly verified.

Keep the type of document in mind. An ADR covers one decision, so judge it only on the requirements that bear on that decision, and mark the others **N/A for this ADR** instead of **Not addressed**.

## 3. Report in chat

1. **Verdict:** one or two sentences on whether the doc is ready, with counts for each rating.
2. **Traceability table:** one row per requirement, with columns for ID, requirement (shortened), rating, and evidence or section.
3. **Gaps:** every **Conflicts**, **Not addressed** and **Partial** item, most serious first, each with a concrete suggested fix.
4. **Scope creep:** anything the doc commits to that no requirement asks for.
5. **Open questions:** any open questions or assumptions in the requirements doc that are still unresolved and that the design depends on.

Then offer to save the report, or to update the doc to close the gaps.
