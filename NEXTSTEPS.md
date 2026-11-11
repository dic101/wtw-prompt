# Next steps — Evergreen assignees (Nov 10, 2026)

Status: sketch only, not filed. Test after the Nov 15, 2026 Sunday scan.
Sketch source: `~/workspace/your_files/wtw-evergreen-assignees-sketch-2026-11-10.md` (local, Nov 10)
Card: WTW-577 — "Test evergreen assignees sketch after Nov 15 Sunday scan — Seat-Model, Dave driving implied" (assignee 👤 Dave — Dave driving, Matty assists on request)
Renumber note (Nov 10, 21:28 ET): filed as WTW-576, but Buffy had already taken 576 on the Mini server (Starwhisker record: 4 Shot moved to Tiff, Extra bomb moved to Avery, #15, 02:26:06Z). The Mini's dedupe guard renumbered this card to WTW-577 on sync (highwater 577, reason dedupe-renumber). GitHub-side 576 file/index/highwater are stale until Dave hits Git Sync on the Mini — do not re-file, the Mini already holds the resolved state.

## Convention, in one line

**A seat is a WHO. A model is a brain inside a seat. A harness is how it runs.**

- Assignee = seat (WHO). Who the card routes *to*. One string, verbatim. Never a model, never a node, never a price.
- Done-by = Seat - Model. Who actually did the work, recorded at completion.
- A model is never an assignee.

## Dave's rulings (Nov 10, 2026)

- Evergreen list lives in `dic101/wtw-prompt/`, updated on Sundays with the Sunday AM model scan. Filers fetch it with `wtw.md` — no workboard repo, no Mini.
- Grow-only: never delete a seat, mark retired/future instead.
- Ask in session if you don't know your seat; if Dave defers, file `⚪ Unassigned`. Never guess nearest, never file `??`.
- 'Otto' is no more. The real identity is Seat (Provider/Harness) - Model.
- **For now, Seat - Model. It implies Dave is driving.** When we get to 'AI driving AI', expand the definition — then the driver is an AI orchestrator, not implied Dave.
- Seat for routing (coarse, stable); model only in Done-by / cost where it actually changes a decision. Don't track every Seat-Model combo — that's too granular.
- Opus-driving-scores-of-Sonnets: probably doesn't matter at Dave's scale. Brief quality dominates, free/cheap workers work, Dave is the checker. Sonnet for frozen-spec builds, Opus held for judgment one-offs. Opus is going away in AGY anyway.
- Variance is the problem: 63 distinct assignee values across 526 cards, 11 Otto variants, Dave in 3 spellings, same model under different names/prices across pickers. Capture was inconsistent, Matty massaged it, outputs weren't the same week after week.

## Next steps — the test (after Nov 15 scan)

How Dave sees next Sunday (Nov 10):

1. Matty reminds Dave to do the stats (Sunday 09:14 nudge). Dave will forget.
2. Eventually Dave dumps the info to the agreed Google Drive.
3. Dave asks Matty to analyze it; Matty analyzes, Dave reviews what Matty comes up with.
4. They come to an agreement.
5. Then Dave asks Matty to make the `assignees.md` file and push to `wtw-prompt`.

So the loop is remind → dump → analyze → review/agree → file, with Dave driving each step and Matty assisting. The frozen capture shape below is a guide for the analysis, not a gate on Dave's dump — the dump is what it is, Matty analyzes it.

For the test itself: derive a draft `assignees.md` from that scan, Seat - Model, Dave driving implied. Two tables, split: Assignees (routes to) and Done-by (records who did it). Propose changes only — one line per seat, source week + evidence — Dave rules in session. No silent adds. No filing to `dic101/wtw-prompt` until Dave says go.

## Open questions for Dave

1. Buffy string/avatar: `🤖 Buffy` — keep, or different avatar?
2. Voice string: `🎙️ Voice` vs `🎙️ Voice (Voiceover Synthesizer)`?
3. DeepAI as a seat: add `🤖 DeepAI`, or models-only?
4. OpenCode as a seat: add, or Done-by only (`OCZ-*`)?
5. Fred: keep + mark `retired`, or drop from the table (keep in Changelog)?
6. Capture template: per-surface table — easy enough for Sunday morning, or too much structure?
7. File name: `assignees.md` vs `roster.md`? (`roster.md` collides in the head with `team-roster.json`.)
8. Handoff file: Dave ruled Nov 10 — `NEXTSTEPS.md` (not `HANDOFF.md`).

## What this does NOT do

- Doesn't file `assignees.md` to `dic101/wtw-prompt` — Dave's go needed, and the `?` rows above need his rulings first.
- Doesn't touch `config/team-roster.json` or board dropdowns — those mirror this file later, if Dave wants that.
- Doesn't change the Sunday scan itself — same surfaces, same nudge, just a frozen capture table + frozen output shape.
