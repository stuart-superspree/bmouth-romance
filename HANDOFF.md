# Romance Writing Festival — Companion App · HANDOFF

_Last updated: 2026-06-17_

A mobile-first web companion for the **Romance Writing Festival** (Sat 3 Oct 2026, Marsham Court Hotel, Bournemouth). Festival-endorsed. Used **before, during and after** the event. Single-file, offline-capable, localStorage only — **no backend yet**.

---

## Files & where things live

| File | Purpose |
|---|---|
| `src/app3.src.html` | **CANONICAL SOURCE** (editable JSX, fully patched). Edit this. |
| `index.html` (repo root) | **DEPLOYED BUILD** (served by Railway at homepage). Self-contained. Build outputs here. |
| `rwf-companion-v1.html` | Root preview copy (same file; gitignored). Keep in sync with `index.html`. |
| `package.json` · `railway.toml` · `serve.json` · `README.md` | GitHub + Railway deploy. Serves repo **root** via `serve .` with `directoryListing:false` + catch-all rewrite to `/index.html`. The old `public/` folder is gitignored. (Railway was serving the root, so the app must sit at the root as `index.html`.) |
| `src/app.src.html`, `src/app2.src.html` | ⚠️ Superseded earlier sources. Do **not** use. |
| `HANDOFF.md` | This file. |

> Note: an old-palette deployed copy may also exist; `rwf-companion-v1.html` is always the live one.

## Build process (IMPORTANT)

The app is **precompiled** — JSX is transpiled and React + Tailwind CSS are inlined so there is **no in-browser Babel and no CDN dependency** (an earlier CDN/Babel approach rendered a blank page). Edits go through a build step, not hand-editing the deployed file.

Build runs in the Linux sandbox (`mcp__workspace__bash`):
1. Copy source → `/tmp/src.html`.
2. Transpile the `text/babel` script with `@babel/core` (preset-react, **classic runtime**).
3. Compile Tailwind via the CLI against a config mirroring the in-source `tailwind.config` (colours/fonts/shadows).
4. Inline `react`/`react-dom` UMD (from npm `umd/` builds) + compiled CSS + custom `<style>` + splash markup.
5. Write the assembled single file to `rwf-companion-v1.html`.
6. Verify headlessly with `jsdom` (mounts, onboarding advances, no runtime errors).

### ⚠️ OneDrive sync caveat (cost us time — read this)
The project folder is OneDrive-synced. After a flurry of `Edit` calls, the **bash mount can read a stale/truncated copy** of a file even though the canonical version (via the `Read` tool) is complete. Symptoms: bash sees a smaller byte count / file ends mid-component.
**Workarounds that work:** (a) write to a **new filename** (fresh paths read correctly), or (b) make edits as **string replacements in the sandbox** (`/tmp`) during the build rather than `Edit`-ing the OneDrive file. Always verify the sandbox copy ends with `</html>` before building.

---

## Design system (locked)

- **Stack:** single HTML file · React 18 (inlined) · Tailwind (precompiled) · shadcn-style components. No build tooling needed to *run*.
- **Palette (exact festival site colours):**
  - Primary **Yellow** `#FFDE59` (brand swatch `#FFCB05`) — **primary** action colour
  - **Sky blue** `#3D9BE9` — secondary
  - **Navy** `#29385F` — secondary / dark surfaces
  - **Ink** `#151C30` — body text · **White** `#FFFFFF` — surfaces/reading
  - **Red** `#ED1C24` — heart accent only
  - Roles: **yellow = primary, blue = secondary** (per Stu, 2026-06-17).
- **Type:** Libre Baskerville (display/titles) + Nunito (UI/body). Uppercase letter-spaced "kickers".
- **Logos:** heart-over-open-book **mark** + stacked **wordmark**, both currently **recreated as SVG/styled text** (real PNGs weren't saved to disk — see backlog).

## Features built (v1)

- **Onboarding** (stage / genre / goal) → personalises the app.
- **Home** — phase-adaptive (before: countdown + key dates + prep checklist; during: now/next + room-finding; after: reflection/plan prompts) + Festival Passport badges.
- **Schedule** — two-track Main-vs-Talks grid (the core "which room?" decision), favourite/attend, ratings, autosaving notes & questions, masterclasses, walks, Friday/evening events.
- **People** — Speaker directory (22 speakers, photos via Wix CDN w/ initial-avatar fallback, conversation starters, save) + **Writer directory** (opt-in mini-profiles, "looking for" filter, save/connect via chosen contact). Seed/local data.
- **Write** — Pitch Builder, Agent Matcher (ranks 6 agents vs your genres), Slush Pile prep (500-word meter), Writing Sprint timer + stats, Flash Challenges, Trope Sparks. (No AI — all curated/deterministic.)
- **More** — Festival guide & FAQs, resource library, reflection, 30/60/90 action plan, progress & connections, profile/settings (incl. phase preview toggle), export summary (.txt).
- **Persistence:** entire state autosaves to `localStorage` (`rwf_companion_v1`) and restores on reopen.

## Key product decisions

- **No AI** — writers are told not to use it; "suggest" features are curated/deterministic only.
- **Festival-endorsed** → official branding used; live directory feasible because moderation/GDPR can sit with the organisation.
- **Networking** delivered as opt-in directory + "looking for" + save/connect (not in-app messaging/forums — too heavy/risky for a 1-day event).
- **Single venue** → no maps; in-hotel room guidance instead.

---

## Backlog / next steps (not yet done)

**High value**
- [ ] **Embed the real logo artwork** — swap recreated SVG/wordmark for the official PNGs (the heart-book mark + full banner). Needs the PNG files saved into `romance-fest/` (or `assets/`); build can base64-inline them.
- [ ] **Live writer directory via Supabase** — currently seed/local. Stand up project + `profiles` table + RLS; no-login "edit-token" model; festival moderates via dashboard. (Supabase MCP tooling is available.) Keep app a single file calling Supabase over HTTPS.
- [ ] **Reminders/notifications** — real push needs PWA + service worker (and/or backend). Currently in-app countdowns/deadlines only.

**Medium**
- [ ] **PWA** — manifest + service worker + icons for installability and true offline (fonts currently load from Google Fonts).
- [ ] Confirm/finalise the **schedule, speaker list and agent wishlists** against the festival's final programme (data seeded from the website June 2026; some may change).
- [ ] Speaker photos currently hotlink Wix CDN — consider caching/embedding for reliability/offline.

**Lower / polish**
- [ ] Coffee Match ("I'm free for 20 min") — deferred (needs real-time + critical mass + backend).
- [ ] Per-speaker tailored conversation starters (currently a shared set).
- [ ] Optional: lightweight analytics of feature use (privacy-respecting).
- [ ] Accessibility pass (contrast on yellow surfaces, focus states, touch targets).

## Verification checklist for any future change
1. Edit `src/app2.src.html` (or successor) — prefer sandbox string-patches if many edits.
2. Build via sandbox pipeline; confirm `/tmp/src.html` ends with `</html>` first.
3. Headless jsdom check: mounts, onboarding advances, no console errors.
4. Re-deploy to `rwf-companion-v1.html`; present file.
