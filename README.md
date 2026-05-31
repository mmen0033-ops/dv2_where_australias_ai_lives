# Where Australia's AI actually lives

FIT2179 Data Visualisation 2, Semester 1 2026.
Author: Munkh-Ochir Mendbayar (Student ID 36564818). Studio 18.

## What this is

A single-page visualisation of where Australia's commercial data centres sit, how submarine cables connect the country to the world, and what the geographic concentration means for users in different regions.

The page is structured in five sections: where the data centres are, why most of them sit in two cities, how submarine cables connect to those clusters, what the resulting latency tax looks like for regional Australia.

## How to run

This is a static site. Open `index.html` in a browser or serve the directory locally:

```bash
python3 -m http.server 8000
```

Then visit http://localhost:8000.

For deployment: push to a GitHub repository and enable GitHub Pages on the main branch.

## File structure

```
.
├── index.html      Main page (CSS inlined, charts embedded via Vega-Lite)
├── sketch.pdf      Hand-drawn sketch
├── README.md       This file
├── charts/         Vega-Lite chart specs as individual JSON files
└── data/           All data files (CSV and JSON)
```

Vega, Vega-Lite and Vega-Embed load from the jsDelivr CDN at runtime.


## Data sources

1. **Cloudscene** — Commercial data centre inventory. cloudscene.com
2. **TeleGeography** — Submarine Cable Map. submarinecablemap.com
3. **Australian Bureau of Statistics** — Regional Population (SA4), June 2024. abs.gov.au
4. **ABS Australian Statistical Geography Standard (ASGS) Edition 3, 2021** — Official SA4 statistical region boundaries and state boundaries, accessed via the `absmapsdata` R package and simplified for web delivery with mapshaper (Visvalingam, retaining shape). All choropleths use these 2021 boundaries. The regional data was matched to the official 2021 SA4 regions by region name, with a point-in-polygon spatial fallback for the small number of records whose labels followed an earlier ASGS edition; one region split across editions was merged by averaging. State polygons were dissolved from the same 2021 SA4 source so every map shares one consistent geographic edition. Metropolitan coastlines for the Sydney and Melbourne insets are derived from open suburb boundary data.

## Interactive and advanced features

- **Bivariate choropleth (latency × population).** Each SA4 region is classified on a 3×3 colour matrix combining round-trip latency with population, each split into terciles, so the regions where high latency and high population overlap (the most people experiencing the worst connectivity) stand out. A custom 3×3 key and a leader-line annotation are included.
- **Site-wide state highlight.** A sticky control bar dims the hero map, the latency choropleth and the bivariate map to a single selected state at once, coordinating three views across the page. It changes opacity only, so no data is ever hidden and the maps still work independently.
- **Animated build-out over time.** A play control advances the data centre map year by year from 2002, with a live running tally, showing how the Sydney/Melbourne concentration emerged rather than simply asserting it. The chart also works as a manual slider.
- **Metro insets with locator and rich tooltips.** The Sydney and Melbourne zooms each carry a small "you are here" national locator and tooltips enriched with ownership and each facility's share of its metro total.

All chart specifications compile as standard Vega-Lite. The interactive controls are wired through vega-embed's view API in `index.html` and degrade gracefully (the animation falls back to its slider, the highlight simply does nothing) if scripting is unavailable.
