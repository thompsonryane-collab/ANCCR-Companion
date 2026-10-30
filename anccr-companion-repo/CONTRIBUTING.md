# Contributing

The companion is a single self-contained `index.html`. Keep it that way.

## Ground rules

- No build step, no bundler, no framework. Plain HTML, CSS and JavaScript.
- No external scripts. Google Fonts is the only permitted remote resource, and every font has a fallback stack.
- Nothing leaves the browser. Do not add network calls, analytics or telemetry.
- Every figure shown must be derivable from the rules in the Sample Dataset tile or from a loaded extract.
- Sensitive data never goes in the repo. The sample is synthetic by design.

## Adding a tile

1. Add an icon to `ICONS` (stroke-based SVG paths, 24×24 viewBox).
2. Add an entry to `TILES` with `id`, `code`, `label` and a two-stop gradient.
3. Add a `COPY` entry with four parts: `what`, `why`, `learn`, `data`. Write for the stakeholder, not the builder.
4. If a pill on the Data tab refers to a table, add it to `DSMAP` and `DSETS` with a schema and a `rows()` sample.
5. Do not add a "new" badge. Badges are reserved for counts.

## Checking a change

```sh
node -e "const fs=require('fs');const m=fs.readFileSync('index.html','utf8').match(/<script>([\s\S]*)<\/script>/);fs.writeFileSync('/tmp/app.js',m[1]);"
node --check /tmp/app.js
```

Then open `index.html` on a phone (or in a narrow browser window) and walk the thirty-minute path in Learn the hub.

## Style

Typographic quotes and apostrophes in prose (’ “ ”); straight quotes in code. Sentence case. No all-caps labels. Plain verbs.
