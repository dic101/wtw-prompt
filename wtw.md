# Filing a Will-the-Workboard card

When Dave asks for a WtW card, do NOT read or browse the workboard repo.

One finished feature set = one card. Split unrelated work into separate cards.
Only file a card when Dave asks — never on your own initiative.

## The JSON

Build a card with exactly these fields:

- `title`: short, imperative. Names the deliverable, e.g. "Wire browser-harness retry loop to the dispatcher's WIP gate".
- `desc`: a self-contained brief a stranger could execute with NO repo access. Include: why (1-2 lines), concrete steps, file paths relative to THIS project root, exact function/file names, and what "done" looks like. Inline any context the worker needs — WtW mesh workers are one-shot model calls with no file access, so never write "see docs/x.md". Keep desc under ~4-5KB: the capability bid reads it in a 4096-token window.
- `dodCheck`: one verifiable string, e.g. `file: src/harness/retry.ts exists`. The endpoint rejects cards without this (or a line starting exactly with `DoD:` / `Verification:` in desc — a `# DoD:` prefix does NOT match) with a 400.
- `acceptance`: plain-language "done looks like", separate from the mechanical dodCheck.
- `grade`: `ordinary` (one clear deliverable, low blast radius) or `careful` (design work, ambiguous scope, or changes touching the whole fleet).
- `project`: this folder's project name (ask Dave if unsure).

## Shaping rules

- **Lane honesty.** If the card requires READING the repo to even scope the work ("find the X", "locate the ..."), say so — it needs a repo-access lane (AGY/Antigravity), not the blind 14b mesh worker. Never write "find the ___" as a hint for 14b.
- **Right-sized.** One deliverable + one mechanical check + fits in a single one-shot = right-sized. If the work needs mid-run decisions based on earlier output, split it.
- **Epics go design-first.** Big features get a design card (deliverable: a design doc), THEN a decomposition card (planner seat turns the design into shovel-ready stories). Never file one giant build card.
- **Blocked cards** name what unblocks them (`Unblocks when WTW-___ is Done.`). Note: nothing auto-unblocks — the flag is cleared by hand.
- **Mobile-first.** UI work targets iPhone width (~390px) as the floor, then progressively enhances for wider screens.

## What a card is (the diary rule)

WtW is the product diary: the logical breakdown of a product (capabilities ->
initiatives -> epics -> features) and the record of RESULTS — conclusions,
pivots, issues found. It is not the technical work itself, and it is not a
mirror of build churn. A card has three moments: creation, a split (if it
splits), and completion.

- **Creation carries the final plan only.** Brainstorms, plan-check audits,
  and rejected options are planning exhaust — never file them. The card
  brief is the frozen plan (or a tight summary + pointers to it, repo +
  artifact paths + frozen SHA, when the plan lives in a repo).
- **Build findings are NOT cards.** Blemishes, blockers, and scope
  discoveries during a build go in the project's GitHub Issues, labelled
  `blemish` / `blocker` / `scope` — issues are a card's interim states.
  Fixes close them from commits (`fixes #N`). Only file a new card when an
  issue outgrows its card: >7 open issues, issues spanning two unrelated
  systems, or one issue that is really a feature (a split).
- **Completion is a summary, not a log dump.** A completion or record card
  (for work already done) states: what shipped, the blemishes that
  mattered, what carries forward, and the receipts (commit SHAs, issue
  numbers). Record cards for finished work are legitimate — mark them
  clearly as records so they can be moved straight to Done.

## Filing

File it with: `curl -X POST http://100.101.43.126:3333/cards -H 'Content-Type: application/json' -d '<json>'`
The response is `{"card": "WTW-<n>"}` — report the card number back to Dave and stop.

Rules:
- No absolute paths, no cross-repo references, no human-gate language ("Dave approves", "Dave reviews").
- Never pick a card number — the endpoint assigns it.
