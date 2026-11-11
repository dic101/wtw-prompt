# Session — filing prompt gaps (2026-11-10)

Work on `dic101/wtw-prompt` issue #1: *Filing prompt gaps: assignee undefined, no
record-card shape, no filing-after-done guidance*. The whole change lives in
`wtw.md`, the prompt that tells an agent how to file a WtW card.

## What shipped

`wtw.md` gained four things:

1. **`assignee` in the field list** — documented as the seat the card routes to,
   defaulting to `⚪ Unassigned`.
2. **`## Assignee: who the card routes to`** — the meaning (routing target, not
   credit for filing), the board picker's 13 canonical single-seat values, a
   warning against copying legacy decorated or compound strings
   (`🤖 Otto (Coder)`, `📋 Will + 🤖 Otto`), and when to set a seat, leave it
   Unassigned, or ask Dave once for an immediately-dispatched card.
3. **`## Record cards: filing work that is already done`** — the shape for
   paper-trailing finished work: `column: done`, `completedAt`, `tag: "Shipped"`,
   a `Record: …` title, receipts in `desc`, a `dodCheck` aimed at the end state,
   and the `{"card": { ...record }}` wrapper the endpoint requires.
4. **`## Reading and editing (API surface)`** — read `GET /api/cards`, create via
   `POST /cards` or `POST /api/cards`, update with `expectedRev`, archive, and the
   team roster endpoint.

## The API finding behind item 3

`POST /cards` is the filing route but hardcodes `column: 'inbox'` and drops
`completedAt` and `rev`. So a record filed there can never land in Done — it sits
in Inbox waiting for a human, which is the exact complaint in the issue.
`POST /api/cards` preserves the `column` and `completedAt` sent with the card, so
records must go through that route instead. The doc states this explicitly rather
than leaving a filer to discover it.

## Git

- Branch: `dic101/filing-prompt-gaps-assignee-undefined-no-record`
- Commit `735b51c` — *prompt: define assignee routing, record-card shape, and filing after done* (`wtw.md`, +95 lines)
- Branch pushed to origin.
- `main` was fast-forwarded to the same commit (`47d9c0f..735b51c`) and pushed.
  A local checkout of `main` was impossible because it is checked out in a second
  worktree, so the remote ref was advanced directly — same result as a merge,
  since the branch was one commit ahead and zero behind.

## Board

Record card **WTW-575** filed straight into `done` via `POST /api/cards`:

- Title: *Record: filing prompt covers assignee, record shape, and filing after done*
- Project `Will the Workboard`, priority `P2`, tag `Shipped`, grade `ordinary`
- Assignee `🤖 Otto` — the implementation seat; precedent is WTW-355 and WTW-183,
  and `📚 Ed` was rejected because this is board tooling, not reference docs
- Receipts in `desc`: commit `735b51c`, branch name, issue #1

## Verification

- The JSON template in `wtw.md` parses (`json.loads`).
- `git diff --check` clean before committing.
- Board re-read after filing: card exists once, `column: done`, `rev: 1`,
  `completedAt` stamped at filing time.
- No test cards were created during the exploration; board access was GET-only
  until the record card was filed.

## Open

- Issue #1 is still open. Its board-side asks are untouched: echo the card back on
  create, publish API docs, and a filing helper script.
- WTW-572 and WTW-573, record cards filed before this prompt change, are still
  sitting in `inbox` — the failure mode item 3 now prevents.
