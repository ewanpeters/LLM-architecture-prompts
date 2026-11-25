# LLM Architecture Prompts

A small library of reusable prompts for using an LLM (Claude, ChatGPT, etc.) to move from a rough product idea to a reviewed solution architecture doc — without rebuilding the same scaffolding every time.

## The workflow

The prompts follow one path, each stage feeding the next:

```
1. Requirements Capture  →  2. Options Analysis  →  3. ADR  →  4. Solution Design Doc
   (what's actually needed)   (what are the choices)   (what we picked, why)   (how it's built)
```

You don't have to run all four for every piece of work — grab whichever stage matches where you are.

## Repo structure

```
prompts/
  01-requirements-capture.md          Turn a rough brief into structured requirements
  02-options-analysis.md              Generate and compare solution options
  03-architecture-decision-record.md  Record the chosen option as a formal ADR
  04-solution-design-doc.md           Write the full solution architecture doc
examples/                             Worked sample output for each prompt
standards/
  review-questions.md                 Your team's standard review questions
.claude/skills/                       Claude Code skills (see below)
```

## Claude Code skills

If you use Claude Code in this repo, these skills load automatically. Run them with `/<name>`, or just describe the task.

| Skill | What it does | Example |
|---|---|---|
| `arch-analysis` | Analyses an architecture and draws a Mermaid diagram | `/arch-analysis` on a codebase |
| `create-adr` | Writes a numbered ADR to `docs/adr/` using the ADR prompt | `/create-adr We chose real-time aggregation over nightly batch` |
| `create-hld` | Writes a high-level design doc to `docs/hld/` using the design doc prompt | `/create-hld for the order-history service, audience: engineers` |
| `review-requirements` | Checks an ADR or HLD against requirements, one requirement at a time | `/review-requirements docs/hld/order-history.md against requirements.md` |
| `review-standards` | Checks an ADR or HLD against `standards/review-questions.md` | `/review-standards docs/adr/ADR-001-...md` |

Fill in `standards/review-questions.md` with your own questions before using `review-standards`. Until then, it only contains example questions and the skill won't run.

## How to use a prompt

1. Open the relevant file under `prompts/`.
2. Copy the prompt block into your LLM of choice.
3. Fill in the `[BRACKETED PLACEHOLDERS]` with your real context — the more specific the input, the better the output.
4. Treat the output as a first draft: push back, ask follow-up questions, and edit before it goes in front of anyone else.

Each file also has a short **Tips** section with non-obvious advice for getting more out of that particular prompt.

## Contributing / adding a new prompt

- One prompt per file, named `NN-short-name.md` so the workflow order stays obvious.
- Every file should have: a one-line "use when" description, the prompt itself, and a Tips section.
- Use `[BRACKETS]` for anything the user needs to fill in.
- Keep prompts provider-agnostic — nothing specific to one vendor's API.

## License

MIT — see [LICENSE](./LICENSE).
