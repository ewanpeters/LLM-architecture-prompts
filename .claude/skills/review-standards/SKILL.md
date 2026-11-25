---
name: review-standards
description: Review an ADR or HLD against the team's standard review questions in standards/review-questions.md. Use when the user asks to "review this against our standards", "run the standard questions", "check this ADR/HLD against the checklist", or wants a design review before sign-off.
---

# Review Against Standard Questions

Answer the team's standard review questions for an ADR or HLD, using evidence from the document. This is a review only: don't edit the document being reviewed.

## 1. Load the questions

Read `standards/review-questions.md`.

- If the file is missing, stop and tell the user to create it.
- If every question is still marked `[EXAMPLE]`, or there are no questions, stop and ask the user to add their own questions first. Don't review against example questions.
- Skip any question still marked `[EXAMPLE]`.

Each question is tagged `[ADR]`, `[HLD]` or `[Both]`. Only use questions that apply to the type of document being reviewed.

## 2. Gather the document

Get the ADR or HLD to review from the arguments, the conversation, or by asking. Work out whether it's an ADR or an HLD from its structure. If you can't tell, ask.

## 3. Answer each question

Give each applicable question one of these ratings:

| Rating | Meaning |
|---|---|
| **Yes** | The doc answers it clearly. |
| **Partial** | The doc touches on it, but the answer is vague or incomplete. |
| **No** | The doc doesn't answer it, or answers it badly. |
| **N/A** | The question doesn't apply to this design. Give a reason. |

Answer from what the doc actually says, and cite the section. Don't give credit for things the doc doesn't say, even if they're likely true.

## 4. Report in chat

1. **Summary:** one or two sentences on overall readiness, with counts for each rating.
2. **Results table:** grouped by the headings in the questions file, with columns for question number, question (shortened), rating, answer, and evidence or section.
3. **To fix:** every **No** and **Partial** item, most important first, each with a concrete suggestion for what to add or change.

Then offer to save the report, or to update the doc to address the gaps.
