# AI Footprint Calculator (class project)

A customized version of Andy Masley's [AI prompt footprint calculator](https://andymasley.com/visuals/ai-prompt-footprint/).

## Layout

- `original/ai-prompt-footprint-source.txt` — the calculator's source exactly as downloaded from Andy Masley's site (`https://andymasley.com/visuals/ai-prompt-footprint-source.txt`), unmodified. Keep this file untouched; it's the reference copy we diff against.
- `calculator/index.html` — a standalone, runnable adaptation of that source. See "Adaptations" below for exactly what changed and why.

## Adaptations from the original

The downloaded source is an [Astro](https://astro.build) component built for andymasley.com's own site (it imports that site's shared page `<Base>` layout and relies on CSS variables defined by that site's global stylesheet — neither of which is included in the download). The calculator's actual markup, script, and CSS are otherwise plain HTML/vanilla JS/CSS with no Astro templating inside them, so no build tooling is needed to run it standalone. Three changes were made to `calculator/index.html`:

1. Replaced the site's `<Base>...</Base>` layout wrapper with a plain `<!doctype html><html><head>...<body>` shell.
2. Defined the 13 CSS custom properties the stylesheet references but doesn't itself set (`--text`, `--text-secondary`, `--dim`, `--border`, `--border-strong`, `--bg-subtle`, `--bg-elevated`, `--accent`, `--accent-hover`, `--accent-subtle`, `--font-body`, `--font-editorial`, `--max-width`) with reasonable default values. These come from andymasley.com's global site CSS, which isn't part of the downloadable source — the values here approximate the live site's look but aren't a byte-for-byte match.
3. Removed the two Astro-only tag directives (`is:inline define:vars={{}}` on `<script>`, `is:global` on `<style>`), which have no meaning outside an Astro build. The tags' content is untouched.

No calculation logic, HTML structure, or CSS rules were altered.

## Features added beyond the original

### Work schedule calculation

The original always annualizes daily AI use with a flat `× 365`. In practice most people don't use AI on every calendar day — many only use it on workdays. The calculator now has two additional inputs in the "Your AI use" panel:

- **AI-use days per week** (1–7, default 7)
- **Weeks worked per year** (1–52, default 52)

These multiply together into `AI_DAYS_PER_YEAR`, which replaces the flat 365 everywhere an *AI-use* figure is annualized: the "over a year" summary, the words/code-lines-per-year figures, the miles-equivalent line, the annual comparison-bar charts, and the cited report. Defaults (7 × 52 = 364) are a near-no-op, so existing baseline numbers only shift once you change the inputs.

The user's personal-lifestyle footprint (home, driving, diet, flying) is unaffected — it still divides by the real 365 calendar days, since that isn't tied to a work schedule. This keeps the headline "% of your daily footprint" comparison anchored to an actual day.

Both inputs persist to the URL (`dw`, `wy` params) alongside the existing shareable state, and reset to defaults with the "Reset" button.

## Running locally

No build step or dependencies required — it's a static HTML file.

```bash
cd calculator
python3 -m http.server 8000
```

Then open http://localhost:8000/ in a browser.
