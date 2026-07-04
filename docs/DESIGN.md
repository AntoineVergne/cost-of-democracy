# Design & Architecture — Cost of Democracy (v2)

The contract between intent (`README.md`) and code. v2 is a **ground-up rebuild** that takes
the predecessor MVP (cost-of-democracy.missionspubliques.tech) as inspiration only and fixes
its five biggest gaps.

---

## 0. What changes from the predecessor MVP

The MVP nailed one math principle we **keep and extend**: **volume ∝ money** (∛amount, "1
voxel = €1M"). We evolve its static *voxel cube* into a **single zoomable Blob Space**
(reusing the 3dpolitics `fractal-blob` concept) where every value sits at its true relative
size at a fixed anchor (**1 px radius = €1M**). Democracy is *physically* a tiny blob beside
trillion-euro blobs, and you navigate by zoom.

The MVP's five gaps v2 fixes (user-confirmed priorities):
1. **Human-scale units** — add €/citizen + "days of median salary" (MVP had only nominal/PPP).
2. **Time normalization** — per-year rate view (MVP mixed 40-yr programs with 1-yr costs).
3. **Global + globe view** — ~12-20 countries, not just France/Poland.
4. **Guided scrollytelling tour** — a narrative path, not a blank scene.
5. **Credibility/source layer** — source + "as of" on every blob; methodology page.

Plus a sixth, cross-cutting priority that shapes everything below:
6. **Visual Capitalist-grade illustration output** — not just a 3D toy; a generator of
   polished, shareable, report-ready infographics.

---

## 1. Guiding principles

1. **Numbers are the product.** Every figure traceable to a cited source + retrieval date.
2. **No backend for v1.** Static SPA; data committed as JSON; deploy to GitHub Pages.
3. **Comparability > completeness.** 12 deep countries beat 180 ragged ones.
4. **Honesty over impact.** Every democracy figure includes its *full running cost*; time and
   population are always normalized before comparison.
5. **Illustration-grade visuals.** Interactive 3D *and* exportable 2D infographics share the
   same math. If it isn't beautiful enough to screenshot into a report, it isn't done.

---

## 2. The aesthetic — Visual Capitalist-grade

The bar is Visual Capitalist: rich, categorized color systems, clean card/tile layouts,
strong typographic hierarchy, small per-item iconography, and dense-but-legible comparison
compositions. Two outputs share one design system:

- **Interactive (web):** the 3D voxel scenes (Arena, Globe, Tour).
- **Illustration (export):** generated 2D infographic panels ("Snippets") — the *same* volume
   math rendered as polished static art for slides, reports, and social posts.

**Design system is a first-class deliverable** (palette, type scale, category color map,
icon set, chart primitives). Build it before features.

**Existing brand kit to reuse** (from the SCALEDEM / 3D Politics family on this machine):
- Written guides: `/home/antoine-vergne/Vibercoder/3dpolitics.xyz/3DPolitics_Style_Guide_v2.md`
  + `3DPolitics_Visual_Mood_Board.md`.
- Implemented tokens (source of truth): `/home/antoine-vergne/Vibercoder/3dpolitics-theme/style.css`
  (`:root`), fonts in `header.php`, illustrations in `assets/images/essays/`.

The kit offers **two directions**. **Decision (locked): hybrid — brutalist app +
watercolor heroes.** The interactive app (Arena, Globe, Tour, Snippets data charts) uses the
**brutalist UI** direction for chrome and data; the **Solarpunk Watercolor** direction is
reserved for **hero / marketing illustrations only**, never for the data charts themselves
(which must stay data-readable and Visual-Capitalist-dense).

1. **App chrome + data charts — "Swiss institutional brutalism" (source of truth =
   `3dpolitics-theme/style.css`):** cream `#F5F1E8` / ink `#1A1A1A` / hazard-rust `#C0392B`;
   **IBM Plex Mono** body + Helvetica-grotesque display + IBM Plex Serif prose. Category
   color map borrows the kit's pillar colors as anchors (Decision `#C0392B`,
   Decentralization `#2E86AB`, Distribution `#5D9B4B`, Synthesis `#7B4F8F`), extended with a
   muted ramp for the Visual-Capitalist density.
2. **Hero / marketing illustrations only — "Solarpunk Watercolor / Ligne Claire":** Solar
   Gold `#D4A017`, Verdant Green `#7A9E7E`, Cyan Future `#5F9EA0`, Terra Cotta `#C65D3B`,
   Twilight Indigo `#4A5568`, Dawn Blush `#E8A598`; sepia lines, never pure black.

⚠️ **Conflict resolved:** pillar palette differs across `style.css`,
`illustration-style-guide.md`, and `recoux.md`. **`style.css` is the single source of truth**;
the others are ignored for color values.

---

## 3. The visualization: four views, one data layer

All views consume the same normalized records (§7), the same cross-cutting controls (§4),
and the **same scaling law**.

### Scaling law (locked — the heart of the project)
- **Primitive: SPHERE / blob** (brand-consistent with the 3dpolitics `fractal-blob`).
- **Volume ∝ money.** A blob's 3D volume is directly proportional to its value. (For a
  sphere V = (4/3)πr³, so volume ∝ money holds exactly; the constant (4/3)π is irrelevant to
  proportionality. Voxels are *not* countable as with cubes — the claim is proportional, not
  integer-exact. Accepted.)
- **radius ∝ ∛money.** €1M → r=1 unit, €1B → r≈10, €1.6T → r≈117. (~10× radius per 1000×
  money — the cube-root compresses scale; accepted tradeoff for honesty.)
- **Two coupled anchors (both always shown):**
  1. **Money anchor:** 1 unit-radius = €1M (the abstract proportional claim).
  2. **Real-world anchor (locked): a €1M blob has a 1.7 m diameter — a person.** So
     `radius_metres = ∛(amount_M) × 0.85 m`. Democracy items render person- to room-sized;
     trillion-euro items render stadium- to district-sized. A **human silhouette (1.7 m)** is
     always present for visceral scale, plus building/stadium silhouettes against the big blobs
     (Demonocracy-style money-next-to-landmarks).
- **Zoom is the navigation.** The scene spans ~1.5 m (democracy) to ~180 m (AUM) radius, so
  zoom is *real navigation* across orders of magnitude. A live readout always shows the truth:
  **"view width ≈ X m ≈ €Y across · zoom Z×"**.
- **Prototype built:** see `prototype/blobs.html` (Three.js, sphere geometry, OrbitControls,
  person reference, live HUD, hover tooltips).

### View A — Blob Space (interactive 3D, hero view)
A single continuous zoomable canvas of **volume-honest sphere blobs** anchored to real-world
metres (€1M = 1.7 m, a person). Democracy blobs (green) sit beside reference blobs (AI gold,
defense rust, wealth purple), each at its true ∛ size — person- to stadium-sized. Because
volume ∝ money, the democracy blob is *visibly* tiny next to a trillion-euro one. A human
silhouette and building/stadium outlines give visceral scale. Hover → tooltip (value,
diameter in m, ratio vs €1M person, source). Add/remove blobs live; **scroll to zoom**
(camera dolly across orders of magnitude), drag to orbit.

### View B — Globe (interactive 3D)
A **globe.gl** globe. Each country carries **two stacked volumes**: democracy-cost (base,
blue) and a chosen reference (top, red) — same ∛ scaling law per country. Rotate, click a
country → breakdown panel. Answers "where is democracy funded relative to need?"

### View C — Scrollytelling Tour (interactive 3D + narrative)
A curated, scroll-driven **zoom path** through the Blob Space for first-time visitors:
**democracy baseline → AI CAPEX → defense → wealth → megaprojects → "your share" reveal.**
The camera dollies between blob groupings, reframing the zoom anchor each step; each step
shows the running ratio and a one-line takeaway. This is the *persuasion* layer; the other
views are exploration.

### View D — Snippets (illustration export, Visual Capitalist-grade)
A generator for **polished static infographic panels**: pick items + unit + layout (stacked
comparison, category grid, "if the budget were €100", timeline). Renders to high-res PNG/SVG
for reports/slides/social. **Same ∛ volume math** as the 3D scenes, rendered as beautiful 2D
(with a visible scale bar so the export stays honest without interactivity). *This view
carries as much weight as the 3D — it is the shareable face of the project.*

---

## 4. Cross-cutting controls (apply to all views)

- **Unit toggle:** Nominal € · €PPP · **€/citizen** · **days of median salary**.
  (MVP had only the first two; the last two are the persuasive differentiators.)
- **Time normalization — single canonical unit: per year.** No total/per-year toggle.
  Everything is expressed as an **annual** value (F-35 program per year, election admin per
  year, parliament per year…) so comparisons are always apples-to-apples.
  - **One-off hardware** (a single jet, a carrier) → **amortized over expected service life**
    and labeled "X per year over N-year life" (e.g. F-35 lifecycle ÷ 40y).
  - **Stocks** (net worth, AUM) → **never forced into a fake rate.** Rendered as
    "% of one year of [reference flow]" (e.g. "Arnault's wealth ≈ N years of France's
    election administration"), clearly labeled as a ratio, not an annualized cost.
- **Country/region selector** (powers per-capita, salary-days, and globe focus).

---

## 5. Credibility / source layer

- Every blob/panel shows **source name + URL + "as of" date** on hover and in export footers.
- Public **/methodology** page: how figures are chosen, normalized, and PPP-converted.
- **/sources** page: every figure, filterable, with retrieval date and license.
- No anonymous numbers anywhere in the UI or exports.

---

## 6. Tech stack

| Layer | Choice | Why |
|---|---|---|
| **3D rendering** | **Three.js** + **globe.gl** (globe view) | Blob Space via Three.js (volume = sphere ∝ ∛money, dolly-zoom nav); globe.gl for the country-globe view. MIT. |
| **2D illustration export** | **D3 + Satori / html-to-image** (or Recharts for dev) | D3 for precise infographic layouts; Satori for PNG/SVG export of React-ish markup. |
| **Framework** | **Vite + TypeScript** (React if UI grows) | Light, fast, deploys to Pages. |
| **Styling / design system** | Tailwind + design tokens (JSON) | Visual Capitalist-grade consistency. |
| **Data** | Committed JSON built from CSV/YAML | Auditable, diff-able, no runtime DB. |
| **Data build** | Node/TS script over source files | Emits normalized `data.json` + `countries.json`. |
| **Testing** | Vitest (normalization math) + Playwright (smoke) | Normalization is the highest-risk code — test it hard. |

---

## 7. Data model

```jsonc
{
  "id": "fr-2024-election-admin",
  "label": "France election administration",
  "category": "democracy",        // democracy | ai | defense | infrastructure | wealth | corporate
  "country": "FRA",               // ISO-3
  "year": 2024,
  "currency": "EUR",
  "amount": 500000000,            // raw, source currency
  "amountEUR": 500000000,         // nominal EUR
  "amountEUR_PPP": 500000000,     // PPP-adjusted to EU27 base
  "itemType": "flow",             // flow | oneoff-hardware | stock  — drives time handling
  "durationYears": 1,             // flow: reporting period; oneoff-hardware: service life
  "amountPerYearEUR": 500000000,  // flow: amountEUR/duration; oneoff-hardware: amountEUR/serviceLife
                                  // stock: UNDEFINED (see ratioToFlowId instead)
  "ratioToFlowId": null,          // stock only: reference flow id for "% of one year of X"
  "ratioYears": null,             // stock only: e.g. 320 => "≈ 320 yrs of [reference]"
  "population": 68000000,
  "perCitizenEUR": 7.35,          // amountPerYearEUR / population (flows + hardware only)
  "medianSalaryDays": 0.12,       // perCitizenEUR / (medianAnnualSalaryEUR/365)
  "source": { "name": "...", "url": "...", "retrieved": "2026-07-01", "license": "..." },
  "notes": "..."
}
```

**Time rules by `itemType` (locked in §4):**
- `flow` → `amountPerYearEUR = amountEUR / durationYears`.
- `oneoff-hardware` → `amountPerYearEUR = amountEUR / serviceLifeYears`; UI labels
  "X/yr over N-year life".
- `stock` → **no `amountPerYearEUR`**; rendered as `ratioYears` against `ratioToFlowId`,
  labeled "≈ N years of [reference flow]". Never mixed into per-year blobs without the label.

`countries.json`: ISO-3, name, lat/lng centroid, population, median income, PPP factor.

**Normalization is highest-risk → most-tested.** Tests encode *why* — e.g. "a `stock` must
never populate `amountPerYearEUR` (would fabricate a fake rate)"; "a 40-yr `oneoff-hardware`
must divide by 40, not compare at face value against a 1-yr democracy flow".

---

## 8. Project structure (target)

```
cost-of-democracy/
├─ README.md
├─ docs/DESIGN.md
├─ design-system/        # tokens (palette, type, category colors), icons
├─ data/{raw, build, sources.csv}
├─ scripts/build-data.ts
├─ src/
│  ├─ main.ts
│  ├─ views/{arena,globe,tour,snippets}.ts
│  ├─ viz/{blobs, infographic, export}.ts   # shared rendering + PNG/SVG export
│  ├─ data/              # load + types
│  └─ ui/                # controls, tooltips, methodology, sources
└─ tests/
```

---

## 9. Phased roadmap

### Phase 0 — Foundations (done)
- [x] Intent, thesis, comparables, sources, v2 scope vs MVP.

### Phase 1 — Minimum Persuasive Product
**Goal:** Blob Space + Snippets (illustration) for **one country**, deployed.
**Done when:** visitor zooms through democracy + reference blobs, switches units (incl.
per-citizen & salary-days) and time (per-year), and can export one Visual Capitalist-grade
panel — every figure with a source.
- Design system (palette/type/category colors/icons) — ported from `3dpolitics-theme/style.css`.
- Vite + TS + Three.js; **blob primitive** (sphere, volume ∝ money, radius ∝ ∛money, anchor
  1 px = €1M) + scroll-to-zoom camera dolly with live "1 px = €X" readout.
- Hand-collected ~8 vetted items with citations.
- Snippets generator (D3 + export).

### Phase 2 — Narrative + credibility
**Goal:** Scrollytelling Tour + methodology/sources pages.
- Tour scene choreography; per-step takeaways.
- /methodology, /sources.

### Phase 3 — Global + globe
**Goal:** Globe view over ~12-20 countries; data-build script replaces hardcoded JSON.
- Ingest IDEA electoral-cost + World Bank population/income/PPP.
- globe.gl view; click → breakdown.

### Phase 4 — Scale & freshness
- Auto-refresh; broaden coverage; community source contributions.

---

## 10. Risks & mitigations

| Risk | Mitigation |
|---|---|
| Thesis looks cherry-picked | Every democracy figure = full running cost; per-year default; sources on every artifact. |
| Cross-country unit errors | Always normalize via PPP + per-citizen + salary-days; never compare raw €. |
| 3D hurts comprehension | Exact values + ratios always visible; Snippets give a clean 2D alternative. |
| Illustration output looks amateur | Lock the design system first; treat Snippets as a first-class view, not an export afterthought. |
| Time mixing (lifecycle vs annual) | Everything is per-year by default; one-off hardware amortized over service life, stocks shown only as labeled "≈ N yrs of X" ratios — never a fabricated rate. |
| Stock items fabricate a fake annual rate | `itemType: stock` must **not** populate `amountPerYearEUR`; enforced by a unit test. |
| Data staleness | `retrieved` date surfaced on every figure. |
| Scope creep | Hard per-phase caps (Rule 2). |

---

## 11. Definition of done (v1 launch)

- [ ] Blob Space renders ≥1 country, ≥8 cited items as honest ∛amount blobs at the 1 px = €1M anchor, with scroll-zoom + live scale readout.
- [ ] Unit toggle (nominal/PPP/**per-citizen**/**salary-days**) correct; per-year is the only time mode; `oneoff-hardware` amortized, `stock` shown as labeled ratio.
- [ ] Snippets exports ≥3 Visual Capitalist-grade layouts (PNG/SVG) with source footers.
- [ ] Every figure shows source URL + "as of" on hover and in exports.
- [ ] Normalization functions unit-tested for *why*.
- [ ] /methodology explains thesis honestly (no overclaim).
- [ ] Deploys to GitHub Pages from `main`.
