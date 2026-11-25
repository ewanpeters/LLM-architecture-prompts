# CLAUDE.md

Markdown-only prompt library: no code, build, tests or tooling. See [README.md](./README.md) for the workflow and how the prompts are meant to be used.

## Structure

- `prompts/NN-name.md` pairs 1:1 with `examples/NN-name-sample.md`.
- `NN` follows the workflow order: 01 Requirements Capture → 02 Options Analysis → 03 ADR → 04 Solution Design Doc.

## Prompt file format

1. `# Prompt: <Name>`
2. `**Stage N of M** — <what it produces>.`
3. `**Use when:** ...`, linking the previous stage where relevant.
4. `## Prompt`, with the prompt in a fenced code block. Use `[BRACKETED PLACEHOLDERS]` for anything the user fills in.
5. `## Tips`: non-obvious advice, a link to the next stage, and a final `See a [sample output](../examples/NN-name-sample.md)` line.

## Example file format

- `# Sample Output: <Name>`, then an intro line linking back to the prompt (`../prompts/...`).
- `## Sample input` (or `## Sample input brief` for stage 1), then `## Sample output`.
- Every example continues one running scenario: a unified customer order-history view for support agents, built on three upstream systems (an on-prem OMS, a SaaS ecommerce platform and a fulfillment partner). Later examples build on earlier ones, so keep new or edited examples consistent with that thread.

## Rules when editing

- Keep prompts provider-agnostic, with no vendor-specific APIs or features.
- Adding, removing or reordering a stage means updating every `Stage N of M` line, the cross-links between prompts, the file numbering, and the README's workflow diagram and repo-structure listing.
- If a prompt's requested output headings change, update its sample output to match.
- After any rename, check that relative links (`./`, `../prompts/`, `../examples/`) still resolve.
