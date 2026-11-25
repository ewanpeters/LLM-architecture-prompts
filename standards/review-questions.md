# Standard Review Questions

The `/review-standards` skill reviews ADRs and HLDs against the questions in this file. Edit it to match your team's standards.

## How to write questions

- Group questions under `##` headings. The headings become the sections of the review report.
- Number each question and tag it with what it applies to: `[ADR]`, `[HLD]` or `[Both]`.
- Make each question answerable from the document itself, e.g. "Does the doc state the RTO and RPO?" rather than "Is this resilient?".
- Questions marked `[EXAMPLE]` are ignored. Replace them with your own, and delete the `[EXAMPLE]` tag from any you want to keep. The skill won't run until at least one real question exists.

## Security

1. [EXAMPLE] [HLD] Does the doc identify where personal or sensitive data is stored and processed, and how it's protected in transit and at rest?

## Operability

2. [EXAMPLE] [HLD] Does the doc describe how the system is monitored and what alerts on failure?

## Decision quality

3. [EXAMPLE] [ADR] Does the ADR state at least one realistic alternative and a specific reason it was rejected?

## Cost

4. [EXAMPLE] [Both] Does the doc estimate the running cost, or name the main things that drive it?
