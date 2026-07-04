# Cost of Democracy

> **Thesis:** Democracy is cheap. The machinery that keeps a society free — elections,
> parliaments, courts, electoral commissions, public broadcasters — costs a tiny fraction
> of what the same society spends on everything else. This project makes that gap *visible*.

## What this is

An interactive 3D data-visualization **and illustration** tool that places the **cost of
democratic infrastructure** side-by-side with the **cost of ordinary things** — a fighter jet,
a year of tax breaks, a sports stadium, a coffee habit, a CEO bonus — so the viewer can *see*
the ratio and conclude for themselves that democracy is one of the best bargains in public
spending.

> **Predecessor (inspiration only):** an MVP at
> `cost-of-democracy.missionspubliques.tech` proved the core math — **volume ∝ money** (∛).
> v2 evolves its static voxel cubes into a **single zoomable Blob Space** (reusing the
> 3dpolitics `fractal-blob` concept) where every value sits at its true relative size at a
> fixed anchor (**1 px radius = €1M**) and you navigate by zoom. It also fixes the MVP's gaps:
> human-scale units, time normalization, global coverage + a globe, a guided tour, a source
> layer, and **Visual Capitalist-grade illustration export**. See
> [`docs/DESIGN.md`](./docs/DESIGN.md) §0.

The unit of comparison is intentionally flexible: dollars, **cost-per-citizen**, or
**"how many days of an average salary."** Normalizing to *per citizen* is what makes the
argument land — it answers "is democracy worth it?" in a unit anyone can feel.

---

## The core comparison (the "wow" frame)

For every country / region we compare two 3D towers:

| Category | Examples | Typical magnitude |
|---|---|---|
| **Democratic infrastructure** *(the cheap blob)* | running an election, funding parliament, the judiciary, the electoral commission, anti-corruption bodies, public-service media | fractions of a cent per citizen per day |
| **Reference things** *(the expensive blobs)* | a single F-35, a football stadium subsidy, a corporate bailout, annual lobbying spend, a year of coffee for the nation | orders of magnitude larger |

The visual story: the democracy blob is a sliver beside blobs of "stuff we pay for without
thinking." **That is the whole point.**

---

## Comparable tools & inspiration (landscape)

Researched during planning. None combine *democracy-cost framing* with *3D volume
visualization* — that gap is this project's reason to exist.

**Civic cost-comparison precedents:**

| Tool | What it does | What we take / differ |
|---|---|---|
| **NPP — Cost of National Security** + **Trade-Offs** (nationalpriorities.org) | Real-time counters of US military spend; "what else those dollars buy." Embeddable widgets. | Strongest direct precedent for *cost comparison*. We extend it from US-only/military to **global + democracy**. |
| **"Death and Taxes" poster** (Jess Bachman) | One beautiful flat infographic of the entire US federal budget. | Inspiration for *completeness*; we trade flat for 3D. |
| **Your Tax Receipt (NPP / national tax agencies)** | Shows where your personal taxes went. | Borrow the *personalization* hook ("your share of democracy costs $X"). |
| **Cost of War (Watson Institute, Brown)** | Cumulative counters + reports on post-9/11 war spending. | A reference for the *expensive* side of the comparison. |

**Visual / volume-scale inspiration (the aesthetic targets):**

| Reference | What it does | What we take |
|---|---|---|
| **Demonocracy.info** | **3D infographics of money as stacks of $100 bills next to real landmarks** (White House, Boeing 747, football field, Statue of Liberty); US debt, budgets, cost of war, all-the-money-in-the-world. | **The closest match to our volume idea.** Proof that *physical volume of money* is the visceral hook. We do volume-as-voxel rather than $-bills, and add democracy as the baseline. |
| **XKCD "Money"** (xkcd 980h) | The canonical hand-drawn wall comparing *everything to everything* (incomes, debts, program costs, GDPs). | Borrow the *breadth* — let users place any item against any item. |
| **Information is Beautiful** (David McCandless) — e.g. "Billion Dollar O-Gram", "Money Map" | Area/category comparison boards; things-as-proportion-of-other-things. | Borrow the *comparison-board* composition for the Snippets/illustration export. |
| **Visual Capitalist** | The polish benchmark: dense, categorized, color-coded infographics. | The **stated quality bar** for exported illustration panels. |
| **BBC / NYT budget viz** ("if the budget were £100") | Normalize-to-100 storytelling. | Borrow *normalize-to-100* + *per-citizen* framing. |
| **Gapminder / Our World in Data** | Trend & comparison dashboards, mostly 2D. | Borrow rigor + open-data ethos. We add the **3D spatial/globe** layer. |

**Gap we fill:** civic cost-comparison tools are (a) US-centric, (b) 2D, (c) rarely put
*democracy* itself in the denominator. This project does all three.

---

## Data sources (all open)

Cost-comparison projects live or die on the credibility of their numbers. Every figure must
link to a source. Planned primary sources:

**Democratic infrastructure costs**
- **International IDEA** — *Electoral Costs* dataset & the "Getting Elections Right" work
  (cost-per-voter across elections globally). The canonical source.
- **OECD** — Government at a Glance (function-of-government spending: general services,
  courts, legislature).
- **World Bank Worldwide Bureaucracy Indicators / PEFA** — public-administration cost data.
- **National electoral commission budgets** ( FEC, Election Commission of India, Elexon/UK
  Electoral Commission, etc.) — scraped/PDF-extracted per country.

**Reference "expensive things"**
- **SIPRI** — military expenditure & arms-transfer data.
- **OECD** — fossil-fuel & agricultural subsidy databases.
- **OpenSecrets** (US lobbying) and national equivalents.
- **World Bank** — GDP, population, GNI per capita (for normalization).
- **Our World in Data** — curated, citable cross-country series.

**Normalization helpers**
- World Bank / IMF — median income, GDP per capita, PPP conversions.

> Principle: store raw + source URL + retrieval date + the derived per-citizen figure, so
> every tower is reproducible and citable. See `docs/DESIGN.md` § Data model.

---

## How to run (once built)

This is the **plan-only** stage — no code yet. The intended stack is documented in
[`docs/DESIGN.md`](./docs/DESIGN.md). Short version: static single-page app, no backend
required for v1 (data committed as JSON), deployable to GitHub Pages.

---

## Project status

- [x] Intent & thesis defined
- [x] Comparable tools surveyed
- [x] Data sources identified
- [x] Predecessor MVP reviewed; v2 scope fixed (zoomable Blob Space + 6 priorities)
- [x] Architecture & 3D/illustration approach designed — see [`docs/DESIGN.md`](./docs/DESIGN.md)
- [ ] Design system (palette, type, category colors, icons)
- [ ] Data pipeline (collect → normalize → JSON) with time + per-capita fields
- [ ] Blob Space (zoomable, volume-honest, 1 px = €1M) + Snippets (Visual Capitalist-grade illustration export)
- [ ] Scrollytelling tour + methodology/sources pages
- [ ] Globe view (~12–20 countries)
- [ ] Public launch

---

## License

To be decided. Data remains under each source's terms; code likely MIT.
