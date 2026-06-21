# Prompt: Options Analysis

**Stage 2 of 4** — generates and compares viable solution options against the requirements.

**Use when:** you have structured requirements (see [Requirements Capture](./01-requirements-capture.md)) and need to explore the solution space before committing to an approach.

## Prompt

```
You are a senior solution architect. Using the requirements below, generate 2-4 viable solution options for solving this problem.

Requirements:
"""
[PASTE OUTPUT FROM REQUIREMENTS CAPTURE, OR YOUR OWN REQUIREMENTS DOC]
"""

Constraints to respect: [TEAM SIZE / TECH STACK / BUDGET / TIMELINE / EXISTING PLATFORM STANDARDS]

For each option:
1. Give it a short name and one-sentence summary.
2. Describe the approach — architecture, key components, how data and control flow.
3. List pros and cons specifically against the non-functional requirements above (not generic pros/cons).
4. Estimate relative cost/effort (low/medium/high) and name the main driver of that estimate.
5. List the key risks and what would need to be true for this option to fail.

Finish with:
- A comparison table: option vs. each non-functional requirement (meets / partially meets / does not meet).
- Your recommendation, with the single biggest reason for choosing it.
- The conditions under which you'd choose a different option instead.
```

## Tips

- Deliberately ask for a "rejected alternative" option (e.g. the obvious-but-wrong answer) — it becomes useful ammunition for the ADR's "options considered" section.
- If the model's recommendation conflicts with your own instinct, ask it to argue the opposite case — disagreements surface hidden assumptions on both sides.
- Carry the comparison table forward into the [ADR](./03-architecture-decision-record.md) prompt.
- See a [sample output](../examples/02-options-analysis-sample.md) for what to expect.
