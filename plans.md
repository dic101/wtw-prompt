# Plans — wtw-prompt

Future work, not started. Items land here when Dave names them; nothing here is a commitment or a build order. Dated Nov 21, 2026.

## Rename the repo (eventual)

- Rename `dic101/wtw-prompt` to something like `dic101/dccgroup-template`.
- Why: the repo now holds the filing prompt plus the evergreen assignee list — the name no longer says what it is.
- Before renaming: update the fetch URL in `dic101/project-template` AGENTS.md (it fetches `https://raw.githubusercontent.com/dic101/wtw-prompt/main/wtw.md`); sweep for hardcoded `wtw-prompt` references in the template, docs, and scripts. GitHub redirects cover old git/web links, but don't rely on the redirect forever.
- Naming note: `dccgroup-template` risks confusion with `project-template` — pick the final name at rename time; it should say what the repo holds.
- After the rename, fold `dic101/flatpack-kit` into the renamed repo (Dave, Nov 21) — the kit (stats service design, vendored client snippets, pattern specs) becomes a directory inside it, so the prompt, the assignees, and the shared libraries live in one place. Sequencing: rename first, then move the kit in.
- Move gotcha: unlike the repo rename, moving files across repos has no automatic redirect — raw URLs pointing at flatpack-kit files will break. The template's "Flatpack kit" pointer section and any docs linking at kit files need updating at move time.

## Port registry reference (landed Nov 23, 2026)

- `wtw.md` now has a "Ports and hosting" section: check the registry, inline the claimed-ports list (mesh workers cannot read the Mini file), default to web-hosted via the Mini reverse proxy (Caddy) with `mac-mini/<service>` → `localhost:<port>`, subdomain per service if the app cannot take a base path.

- Add a port-avoidance section to `wtw.md` (or a linked `PORTS.md`) so a new app knows which ports are already claimed before it picks one.
- Live registry is the canonical copy: `~/ports.md` on the Mac Mini (also `~/workspace/homelab/ports.md` on Matty's side). It lists item, host, port, link, status, health-check curl, and how to launch.
- Catch: mesh workers are stateless and can't read the Mini file — the prompt must inline the claimed-ports list, refreshed when the registry changes. A bare link doesn't reach them.
- Known collision to call out: `:3335` is claimed twice on the MBA side (ChowGym Costco display server now; Easy Button design also says 3335).

## Shared libraries (planned)

- **High scores**: Starwhisker's `highscores.js` (dependency-free, per-game storage key; landed with #10) → shared library for Starwhisker + StrataWords (+ future games). Dave asked Nov 10 whether the keeper can serve StrataWords; the module was spec'd for exactly that. Note: StrataWords already has its own localStorage stats/streak store — adoption is a separate decision per game.
- **Authentication routines**: the Oct 9 auth tiers as reusable patterns — public, no gate; light gate (Mahjong invite-code in link/QR, StrataWords Caddy Basic Auth); identity tier (Caddy `forward_auth` → Authelia with Google Workspace, Cloudflare Access as the alternative). Flatpack-style: vendored or referenced, not copied into project repos.
- Home (Dave, Nov 21): the shared code lives with the kit, and the kit folds into the renamed repo — see Rename above.

## Shared services hosting (eventual)

- At some point, shared services live at `http://dccgroup.net` for our apps to call — or the apps call LAN-based services instead. Both paths stay open; the choice is per service.
- The flatpack stats service is the first candidate for a dccgroup.net home (deployed once, called at runtime; games don't pull it in).
- LAN-based stays for private/internal stuff (homelab services on the Mini/MBA/G4/AN1, tailnet-only). This lines up with the Oct 9 auth tiers: public no-gate, light gate, identity tier, no public route at all.
- Note: dccgroup.net itself is currently untouched — nothing live there yet.
