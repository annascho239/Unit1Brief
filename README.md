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

## Running locally

No build step or dependencies required — it's a static HTML file.

```bash
cd calculator
python3 -m http.server 8000
```

Then open http://localhost:8000/ in a browser.
