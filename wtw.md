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
- `assignee`: the seat the card routes to (see below). Defaults to `⚪ Unassigned`.

## Assignee: who the card routes to

`assignee` is the seat the card is routed TO — the seat meant to execute it. It is
not credit for who captured, filed, or approved the card. One card, one seat.

Canonical values (the board picker offers exactly these; use the string verbatim):

| value | seat |
| --- | --- |
| `⚪ Unassigned` | nobody routed yet (default) |
| `👤 Dave` | product direction, architecture, final sign-off |
| `👧 Quinn` | UX critique & playtesting |
| `👩 Mandy` | human playtesting (word/puzzle games) |
| `📋 Will` | workboard intake, triage, decomposition |
| `🤖 Otto` | implementation, code, builds |
| `🎨 Palette` | UI/UX design, imagery & media |
| `🧪 Tess` | QA, probes, reliability |
| `📚 Ed` | docs, specs, archival knowledge |
| `✍️ Quill` | writing, narratives, build logs |
| `💰 Fred` | token/budget treasury |
| `💬 Matty` | conversational & life layer |
| `🔍 Scout` | deep research, landscape scans |

Older cards carry decorated or compound strings (`🤖 Otto (Coder)`, `📋 Will + 🤖 Otto`).
Do not copy those: the picker cannot represent them. If two seats genuinely share
the work, assign the one that executes and name the other in `desc`.

When to set it:

- Set the executing seat when the brief makes the owner obvious (a code fix → `🤖 Otto`,
  a podcast script → `✍️ Quill`, a reference doc → `📚 Ed`).
- Leave `⚪ Unassigned` for raw capture and cards whose scope is not yet known. Do not
  interrogate Dave about routing for an unscoped capture.
- Ask Dave only when the card will execute immediately (`ready`, dispatch) and two
  seats are equally plausible. Ask once, naming the candidates.

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

## Record cards: filing work that is already done

A record card is the paper trail for work that finished before anyone filed it. File
it once, straight into `done`, marked clearly as a record. Do not leave it in Inbox
for a human to move, and do not file a second card for work that already has one —
edit the existing card's fields and column instead.

Board columns are `inbox`, `ready`, `progress`, `done`. File records with
`POST /api/cards` (below), not `POST /cards`: the filing route always creates in
`inbox` and ignores `column` and `completedAt`. The record sets `"column": "done"`,
`completedAt` (ISO-8601 UTC, now), and `"tag": "Shipped"`, and the endpoint stamps
`id`, `ticketId`, `rev`, and `createdAt` for you.

```json
{
  "title": "Record: <what shipped, past tense>",
  "desc": "Record of <what happened>, filed after the fact on <YYYY-MM-DD>.\n\n- <fact the reader cannot infer>\n- <receipt: commit SHA, issue #, URL, amount>\n- <what carries forward, if anything>\n\nDoD: <mechanical proof the end state exists>.",
  "column": "done",
  "tag": "Shipped",
  "project": "<project>",
  "priority": "P2",
  "assignee": "<the seat or person that did the work>",
  "dodCheck": "<same check as the DoD line>",
  "acceptance": "<plain-language done>",
  "grade": "ordinary",
  "completedAt": "<ISO-8601 UTC>"
}
```

Send it wrapped as the `card` value: `{"card": { ...record }}`.

State what shipped and the receipt, not the build log. Keep the date the work
happened in `desc` if it differs from the filing date. Records still need a
`dodCheck` — point it at the end state (an account exists, a domain resolves), not
at a process step. If a human did the work themselves (a purchase, an account
created), `assignee` is that person, e.g. `👤 Dave`.

## Filing

File it with: `curl -X POST http://100.101.43.126:3333/cards -H 'Content-Type: application/json' -d '<json>'`
The response is `{"card": "WTW-<n>"}` — report the card number back to Dave and stop.

Rules:
- No absolute paths, no cross-repo references, no human-gate language ("Dave approves", "Dave reviews").
- Never pick a card number — the endpoint assigns it.

## Reading and editing (API surface)

Base URL: `http://100.101.43.126:3333`.

- `GET /api/cards` → `{"status": "ok", "cards": [...]}` (the whole board, 500+ cards).
  Find one: `curl -s http://100.101.43.126:3333/api/cards | jq '.cards[] | select(.ticketId == "WTW-123")'`.
- `POST /cards` is the filing route (see Filing). It always creates in `inbox`, assigns
  the next `WTW-n`, ignores `column`, `completedAt`, and `rev`, and returns
  `{"card": "WTW-<n>", "number": <n>}`.
- `POST /api/cards` creates or updates one card. Body: `{"card": {...}, "expectedRev": <rev>}`.
  This is a full-card save, not a patch: fetch the card, change fields on that
  object, send the whole thing back. On update, pass the `rev` you read as
  `expectedRev`; the server increments it, and a stale rev merges instead of
  overwriting — never set `rev` by hand. On create, omit `expectedRev`; the server
  assigns `id`, `ticketId`, `rev`, and `createdAt`, and keeps the `column` and
  `completedAt` you send (this is how a record lands straight in `done`). Returns
  `{"status": "ok", "card": {...}}` — read `card.ticketId` for the number.
- `POST /api/cards/<ticketId>/archive` archives a card; it leaves the board and its
  number is not reused.
- `GET /api/team-roster` returns the seats behind the canonical assignee values.
