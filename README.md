# RCQ Macro Command Board

Public macro dashboard for the RC Quantum Automation investment group, served at **https://macro.rcqsignals.com** via Cloudflare Pages.

## How it updates
A scheduled Claude task runs each weekday ~5:47 AM PT. It refreshes the readings and commits new JSON to `data/`. Each commit triggers a Cloudflare Pages deploy. No build step.

## Layout
- `index.html` — the page (static; reads `data/*.json` at load)
- `data/` — `metrics`, `history`, `curve`, `brief`, `regime`, `calendar`, `meta` (last update time)
- `assets/` — RCQ logo layers
- `_headers` — no-cache on `data/` so viewers always get the latest numbers
- `SCHEMA.md` — the data contract for `data/*.json` (machine-readable fields, units, redistribution flags)

## Cloudflare Pages settings
- Framework preset: None · Build command: *(empty)* · Output directory: `/`
- Custom domain: `macro.rcqsignals.com`

Figures are the latest official prints or market closes. Not investment advice.
