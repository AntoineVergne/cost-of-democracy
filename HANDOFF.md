# HANDOFF — Cost of Democracy

**Read this first.** It is the single entry point for any agent (or human) taking over.
It captures intent, current state, every locked decision, open flags, and the exact next
steps — so you can continue without re-deriving anything.

> Last updated: 2026-07-04. Living document — update it as you change state.

---

## 0. TL;DR

- **Project:** *Cost of Democracy* — an interactive 3D + illustration tool that shows
  **democratic infrastructure is cheap** by rendering it as **volume-proportional blobs**
  next to trillion-euro reference items (AI, defense, wealth).
- **Core mechanic (locked):** each item is a **sphere** whose **volume ∝ money**
  (radius ∝ ∛money), anchored to **real-world size**: a €1M blob = 1.7 m diameter = a person.
  Democracy blobs are person-sized; trillion-euro blobs are stadium-sized. Navigate by zoom.
- **State:** planning **complete and decision-locked**; one runnable concept prototype exists
  (`prototype/blobs.html`). **No build tooling, no package.json, no real data yet.**
- **Immediate next action:** start **Phase 1** (§9) — port design tokens → scaffold Vite/TS →
  rebuild the blob primitive properly → collect ~8 sourced items → Snippets export.
- **Primary docs:** [`README.md`](./README.md) (intent + landscape) and
  [`docs/DESIGN.md`](./docs/DESIGN.md) (full architecture). This file synthesizes + adds state.

---

## 1. Intent & thesis

**Thesis:** *Democracy is cheap.* The machinery that keeps a society free — elections,
parliaments, courts, electoral commissions, anti-corruption bodies, public-service media —
costs a tiny fraction of what the same society spends on everything else. The project makes
that gap **visible and felt** by putting democratic-infrastructure costs beside reference
costs (a fighter jet, AI CAPEX, a billionaire's wealth, a megaproject) at honest relative size.

The persuasive unit is **per citizen** and **"days of median salary"** — converting abstract
billions into a felt ratio. See README §"The core comparison".

---

## 2. Current state

| Artefact | Path | Status |
|---|---|---|
| Intent + comparable-tools landscape | `README.md` | done |
| Full architecture & decisions | `docs/DESIGN.md` | done, locked |
| **Prototype v2** (premium 3D + watercolor) | `prototype/blobs.html` | runs; **illustrative data only** |
| Data pipeline / real figures | — | not started |
| Build tooling (Vite/TS/tests) | — | not started |
| Repo-level `AGENTS.md` (run/test cmds) | — | not started — **create when you scaffold** |

**Prototype v2 — what's implemented** (single self-contained HTML, Three.js r0.161 via CDN):
- **Premium 3D mode:** liquid-metal `MeshStandardMaterial` blobs (metalness 0.92, roughness
  0.16), procedural HDRI reflections via `RoomEnvironment` + `PMREMGenerator`, warm radial-
  gradient skydome, `ShadowMaterial` ground, `UnrealBloomPass` (strength 0.7 / threshold 1.0,
  only HDR speculars bloom) + `OutputPass`, ACESFilmic tone mapping, gentle per-blob bob.
- **Watercolor mode (View D preview):** toggled via the top switch. SVG, paper background
  with grain filter, circles at the *same* ∛ scale rendered through a `feTurbulence` +
  `feDisplacementMap` watercolor filter (wet-on-wet layered fills, sepia leader lines,
  IBM Plex Serif italic labels). Honest horizontal-scroll layout.
- **Shared:** one data array + math (volume ∝ money, `radius = ∛(amountM) × 0.85 m`, anchor
  €1M = 1.7 m person), person silhouette, hover tooltips, live HUD (view width ≈ € across),
  legend toggles, camera-focus buttons, brutalist UI panels (cream/IBM Plex Mono/pillar colors).
- **Verified:** serves HTTP 200; all premium tokens present. **Not verified in this env:**
  actual WebGL render (no browser) — open it to confirm visuals.

**Predecessor (inspiration only, do not extend):** the MVP at
`https://cost-of-democracy.missionspubliques.tech`. It proved volume ∝ money with *voxel
cubes*; v2 evolves that into a zoomable **sphere-blob** space with brand consistency.

---

## 3. LOCKED decisions — do NOT re-litigate

These were settled with the user across the conversation. If you disagree, **surface it and
ask**; do not silently change direction (Rule 7).

1. **Primitive = SPHERE / blob** (not cubes). Brand-consistent with the 3dpolitics
   `fractal-blob` concept.
2. **Scaling = volume ∝ money** → radius ∝ ∛money. *(Cube-root compresses scale: ~10× radius
   per 1000× money. The epic "1000× bigger" zoom was explicitly traded away for honesty.)*
3. **Real-world anchor = €1M blob is 1.7 m diameter (a person).**
   `radius_metres = ∛(amount_€M) × 0.85`. Human + building/stadium silhouettes always present.
4. **Aesthetic = hybrid:** brutalist app chrome + data charts (cream `#F5F1E8`, ink, IBM Plex
   Mono, hazard-rust `#C0392B`, pillar colors); **Solarpunk Watercolor reserved for hero /
   marketing illustrations only**, never for data charts.
5. **Time = always per-year.** No total/per-year toggle. Flows use annual amount.
6. **One-off hardware** (a jet, a carrier) → **amortized over service life** ("X/yr over N-yr life").
7. **Stocks** (net worth, AUM) → **never annualized.** Shown only as a labeled
   "≈ N years of [reference flow]" ratio. A `stock` must never populate `amountPerYearEUR`.
8. **Units toggle:** Nominal € · €PPP · **€/citizen** · **days of median salary**.
9. **Four views:** A) Blob Space (hero, zoomable), B) Globe (globe.gl, per-country), C)
   Scrollytelling Tour (guided zoom path), D) Snippets (Visual-Capitalist-grade 2D export).
10. **No backend for v1** — static SPA, data as committed JSON, deploy to GitHub Pages.
11. **Stack:** Three.js + globe.gl + Vite + TypeScript (+ React only if UI grows); D3 + Satori
    for Snippet export; Vitest for normalization math; Tailwind + design tokens.
12. **Credibility layer:** source name + URL + "as of" date on **every** figure (hover + export
    footer); public `/methodology` and `/sources` pages.

---

## 4. OPEN flags — need attention / human input

- **Prototype deviates from locked rules on purpose** (for demo speed): the Rafale is shown at
  **face value** (should be amortized), BlackRock AUM at **face value** (should be a ratio),
  and amounts are **illustrative**, not sourced. The real build MUST apply rules 6 & 7 and use
  cited figures. Don't copy the prototype's data-handling into the real app.
- **Zoom "drama" is mild under ∛** (whole range ~1.5 m → ~180 m radius). User accepted this.
  If it later feels too tame, the fallbacks are **area (√)** or **linear** scaling — but that
  is a user decision, not yours to make unilaterally.
- **No logo exists** in the brand kit (typographic wordmark only). Flag if one is needed.
- **Display font (Helvetica grotesque) is not web-served** in the source theme — falls back to
  system Helvetica. Decide whether to license/serve a real grotesque for v1.

---

## 5. The scaling law — implement exactly this

```
volume ∝ money
radius_units = ∛(amount_in_€M)           // €1M → 1, €64M → 4, €1B → 10, €1.6T → ~117
radius_metres = radius_units × 0.85       // anchor: €1M blob diameter = 1.7 m (a person)
```

| Money | radius (m) | diameter (m) | feels like |
|---|---|---|---|
| €1M | 0.85 | 1.7 | a person |
| €5M | 1.45 | 2.9 | a doorway |
| €64M | 3.4 | 6.8 | a truck |
| €500M | 6.75 | 13.5 | a 4-storey building |
| €1B | 8.5 | 17 | a 5-storey building |
| €35B | 27.8 | 55.6 | a mid skyscraper |
| €1.6T | ~99 | ~199 | a stadium / city block |

⚠️ **Honesty caveat to keep:** a *sphere* makes volume ∝ money **proportionally** but **not
integer-countably** (a radius-4 sphere ≈ 268 unit-volumes, not 64). That's the accepted cost of
choosing spheres over cubes. State it in `/methodology`.

---

## 6. Brand kit — where it is and the rules

The kit is **split across two folders** on this machine (parent brand = SCALEDEM / 3D Politics):

| Role | Path |
|---|---|
| Written style guides | `/home/antoine-vergne/Vibercoder/3dpolitics.xyz/3DPolitics_Style_Guide_v2.md` + `3DPolitics_Visual_Mood_Board.md` |
| **Implemented tokens (SOURCE OF TRUTH)** | `/home/antoine-vergne/Vibercoder/3dpolitics-theme/style.css` (`:root`) |
| Fonts (Google Fonts link) | `/home/antoine-vergne/Vibercoder/3dpolitics-theme/header.php` |
| Illustration library (~30) | `/home/antoine-vergne/Vibercoder/3dpolitics-theme/assets/images/essays/` |
| Signature blob concept | `/home/antoine-vergne/Vibercoder/3dpolitics-theme/assets/js/fractal-blob.js` |

**Palette (from `style.css` — the only source of truth):**
- Surface: bg `#F5F1E8`, paper `#FFFCF5`, ink `#1A1A1A`, muted `#6B6B6B`, accent/hazard `#C0392B`.
- Pillar/category colors: Decision `#C0392B`, Decentralization `#2E86AB`, Distribution `#5D9B4B`, Synthesis `#7B4F8F`.
- Typography: display = Helvetica grotesque (system fallback), **body/UI = IBM Plex Mono**, prose = IBM Plex Serif.
- Hero-only watercolor accents: Solar Gold `#D4A017`, Verdant Green `#7A9E7E`, Cyan Future `#5F9EA0`, Terra Cotta `#C65D3B`.

⚠️ **Palette conflict (resolved — do not reopen):** the pillar colors differ across
`style.css`, `illustration-style-guide.md`, and `recoux.md`. **`style.css` wins**; ignore the
others for color values. Do **not** average them.

---

## 7. Run the prototype

```
cd /home/antoine-vergne/cost-of-democracy
python3 -m http.server 8000
# open http://localhost:8000/prototype/blobs.html
```
Drag to orbit, scroll to zoom, hover blobs for details, use the focus buttons / legend toggles.
**Verified:** serves HTTP 200, libs present. **Not verified in this env:** actual WebGL render
(no browser) — open it to confirm visuals.

---

## 8. Phase 1 — the concrete next steps (in order)

**Goal:** Blob Space + Snippets for **one country**, deployed to GitHub Pages, every figure
sourced. Definition of done is in `docs/DESIGN.md` §11.

1. **Create repo `AGENTS.md`** with the run/dev/test commands once tooling exists.
2. **Port design tokens** from `3dpolitics-theme/style.css` into a `design-system/tokens.json`
   (or Tailwind config). Lock palette + type scale + category color map.
3. **Scaffold Vite + TypeScript** (add React only if the UI around the scene grows). Three.js
   + `globe.gl` as deps.
4. **Rebuild the blob primitive properly** — port `prototype/blobs.html` logic but: read from
   a data file, apply the **per-year + amortize-hardware + stock-as-ratio** rules, keep the
   €1M = 1.7 m anchor, person + building silhouettes, live HUD readout, hover tooltips.
5. **Collect ~8 sourced items** (2 democracy, ~6 reference) for one country, with citations +
   retrieval dates. Store as normalized JSON per the data model in `docs/DESIGN.md` §7.
6. **Unit-test the normalization math** (Vitest) — encode *why* (e.g. "a `stock` must never
   set `amountPerYearEUR`"; "one-off hardware divides by service life, not face value").
7. **Snippets generator (View D)** — D3 layouts + Satori/html-to-image PNG/SVG export, with a
   visible scale bar and source footer. Visual-Capitalist-grade polish.
8. **Deploy** to GitHub Pages from `main`.

Phase 2 (Tour + methodology/sources pages) and Phase 3 (Globe + ~12–20 countries + data-build
script ingesting IDEA/World Bank) are scoped in `docs/DESIGN.md` §9.

---

## 9. Data sources (open, for Phase 3 pipeline)

- **Democratic-infrastructure costs:** International IDEA (Electoral Costs), OECD Government at
  a Glance, World Bank PEFA, national electoral-commission budgets.
- **Reference costs:** SIPRI (military), OECD (subsidies), OpenSecrets (lobbying), company
  filings (AI CAPEX).
- **Normalization:** World Bank / IMF — population, median income, GDP/capita, PPP factors.
Store raw + source URL + retrieval date + derived per-citizen figure for every record.

---

## 10. Gotchas — easy things to get wrong

- **Sphere volume isn't countable** — don't claim "1 voxel = €1M"; say "volume ∝ money".
- **Everything is per-year** — never compare a 40-yr program total against a 1-yr democracy
  budget. Amortize hardware; show stocks only as labeled ratios.
- **Never compare raw € across countries** — always via PPP + per-citizen + salary-days.
- **Source on every figure** — hover AND export footer. No anonymous numbers anywhere.
- **One color source of truth** (`style.css`) — don't blend the conflicting palette docs.
- **Data readability over atmosphere** — watercolor palette is for heroes only, never data
  charts. The VC target means dense, legible, categorized color.

---

## 11. Conventions

- Follow the operator rules in `/home/antoine-vergne/AGENTS.md` (12 rules; notably: think
  before coding, simplicity first, surgical changes, surface conflicts, fail loud, match
  codebase conventions).
- Match the existing doc voice (terse, tables, explicit tradeoffs, ⚠ for flags).
- Don't add code comments unless asked. No emojis in files unless asked.
- Don't commit unless explicitly asked.

---

## 12. File map

```
cost-of-democracy/
├─ README.md              # intent, thesis, comparable-tools + inspiration landscape
├─ HANDOFF.md             # THIS FILE — start here
├─ docs/DESIGN.md         # full architecture, scaling law, data model, roadmap, risks, DoD
└─ prototype/blobs.html   # runnable concept prototype (illustrative data; not the real app)
```
