# Filing a Will-the-Workboard card (end of session)

When Dave asks for a WtW card, do NOT read or browse the workboard repo.

One finished feature set = one card. Split unrelated work into separate cards.

Build a JSON card with exactly these fields:
- `title`: short, imperative. Names the deliverable, e.g. "Wire browser-harness retry loop to the dispatcher's WIP gate".
- `desc`: self-contained brief a stranger could execute with NO repo access. Include: why (1-2 lines), concrete steps, file paths relative to THIS project root, exact function/file names, and what "done" looks like. Inline any context the worker needs — WtW workers are one-shot model calls with no file access, so never write "see docs/x.md".
- `dodCheck`: one verifiable string, e.g. `file: src/harness/retry.ts exists`. The endpoint rejects cards without this (or a line starting with `DoD:` / `Verification:` in desc) with a 400.
- `project`: this folder's project name (ask Dave if unsure).

File it with: `curl -X POST http://100.101.43.126:3333/cards -H 'Content-Type: application/json' -d '<json>'`
The response is `{"card": "WTW-<n>"}` — report the card number back to Dave and stop.

Rules:
- No absolute paths, no cross-repo references, no human-gate language ("Dave approves", "Dave reviews").
- Never pick a card number — the endpoint assigns it.
