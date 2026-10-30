# ANCCR AI Companion

A client-facing companion to the ANCCRai hub (Utility Commodity Command, NAVFAC PW6 Utility Procurement). It keeps the v1.7 hub-tile layout but speaks to the stakeholder rather than the builder: what each module does, why it matters to the user's job, how to learn it in ten minutes, and what data it needs — with the sample data's derivation, sources and formulas shown in full.

Built for iPhone 15 and later; runs in any modern browser. One file, no build step, nothing leaves the device.

## Run it

Open `index.html`. That is the whole app.

For a hosted copy, push to `main` with GitHub Pages enabled (Settings → Pages → Source: GitHub Actions). The workflow in `.github/workflows/pages.yml` deploys the repository root.

On iPhone, open the hosted URL in Safari and choose Share → Add to Home Screen. The manifest and icons give it a standalone launch.

## What is in it

| Tile | Code | Purpose |
|---|---|---|
| Learn the hub | ACR-16 | A thirty-minute path through the tiles, starting from what people already know |
| Sample Dataset | ACR-14 | Where every demo number comes from: derivation, sources with live links, formulas, planted errors |
| Strategy | ACR-15 | The 29 October analysis as a working document: summary, meeting, gaps, plan, actions, mind map |
| ANCCR Analyst | AI-01 | Preset stakeholder questions with the module each answer comes from |
| Portfolio Command … Settings | ACR-01 … ACR-13 | One sheet per hub module: what it does, why it matters, learn it, data |

Every pill on a module's Data tab opens the table it refers to, with a schema view and a sample of rows. Nineteen tables are documented in `docs/data-dictionary.md`.

## Layout

```
index.html                 the app
manifest.webmanifest       home-screen install metadata
assets/icon-*.png          app icons
docs/data-dictionary.md    schema of every sample table
docs/formulas.md           every calculated figure, written out
docs/sources.md            public sources with links; sample at a glance
.github/workflows/         Pages deploy and a syntax check on PRs
CHANGELOG.md, CONTRIBUTING.md, LICENSE
```

## Data

All consumption, price and bill figures are synthetic, generated from the rules shown in the Sample Dataset tile. Installation and utility names are real. Real extracts enter through the Data Drop Zone in the hub itself, not through this companion.

The agreed validation dataset is Naval Weapons Station Yorktown, three years including interval data.

## Related

- ANCCRai hub, v1.7 (present mode) — the platform this companion explains
- 10-29 Meeting: ANNCR AI Data Integration and Validation — the analysis carried in the Strategy tile

## Marking

CUI // FOUO where applicable. Nothing in this repository is sensitive by design; keep it that way.
