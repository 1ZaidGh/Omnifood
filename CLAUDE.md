# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Omnifood is a static, single-page marketing website for an AI-powered meal subscription service. It is pure HTML, CSS, and vanilla JavaScript — no build tools, no package manager, no framework.

## Development

Open `index.html` directly in a browser, or use the VS Code **Live Server** extension (configured to run on port **5501** via `.vscode/settings.json`).

There are no build steps, no tests, and no linting tools.

## Architecture

### CSS — Three-File System

All styles are split across three files loaded in this order in `index.html`:

| File | Purpose |
|---|---|
| [css/general.css](css/general.css) | Design system: typography scale, color palette, spacing system, grid utilities, reusable components (`.btn`, `.link`, `.list`, `.container`, `.grid--*-cols`) |
| [css/style.css](css/style.css) | Section-scoped component styles (header, hero, meals, pricing, CTA, footer, etc.) |
| [css/queries.css](css/queries.css) | Responsive breakpoints only — never add layout here, only overrides |

**Design tokens** are documented at the top of `general.css`:
- **Primary color**: `#e67e22` (orange), tints `#fdf2e9` / `#fae5d3` / `#eb984e`, shade `#cf711f`
- **Font**: Rubik (Google Fonts), base `font-size: 62.5%` on `html` so `1rem = 10px`
- **Spacing scale (px)**: 2 / 4 / 8 / 12 / 16 / 24 / 32 / 48 / 64 / 80 / 96 / 128
- **Border-radius**: 9px (default), 11px (cards/panels)

**Responsive breakpoints** (in `queries.css`, using `em` units):
- `84em` → 1344px: smaller desktops
- `75em` → 1200px: landscape tablets (also scales `html` font-size to 56.25%)
- `59em` → 944px: tablets — mobile nav activates here (`font-size: 50%`)
- `44em` → 704px: smaller tablets
- `34em` → 544px: phones

### JavaScript — [js/script.js](js/script.js)

Three behaviours, all vanilla JS:

1. **Current year** — sets `.year` text content automatically
2. **Mobile navigation** — toggles `.nav-open` class on `.header` when `.btn-mobile-nav` is clicked; the CSS in `queries.css` drives visibility via `opacity`/`pointer-events`/`visibility`
3. **Sticky header** — `IntersectionObserver` watches `.section-hero`; adds/removes `.sticky` on `document.body` with `-80px` root margin

A commented-out flexbox `gap` polyfill for old Safari is also present (adds `.no-flexbox-gap` to body if unsupported; corresponding fallback CSS rules are in the comment block at the bottom of the file).

### HTML — [index.html](index.html)

Single page. Section anchor IDs use the `n-` prefix (e.g. `#n-how-it-works`, `#n-meals`, `#n-testimonials`, `#n-pricing`, `#n-cta`). Navigation links target these.

The CTA form uses `name="sign-up" netlify` — it is wired for Netlify Forms on deployment.

### External Dependencies (CDN only)

- **Google Fonts** — Rubik, loaded via `<link>` preconnect in `<head>`
- **Ionicons 7.1.0** — icon web components loaded via two `<script>` tags at the bottom of `<body>` (ESM module + nomodule fallback)

### Images

- Hero image uses `<picture>` with `.webp` source and `.png` fallback
- Favicons provided at multiple sizes (`favicon-64.png`, `apple-touch-icon.png`)
- PWA manifest at `manifest.webmanifest`
- CTA background image (`img/eating.jpg`) is referenced directly in `style.css` via `url(../img/eating.jpg)`
