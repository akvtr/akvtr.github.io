# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Stack

Plain HTML + CSS — no frameworks, no build step, no JavaScript unless explicitly noted. Fonts are loaded via Google Fonts: **JetBrains Mono** (headings, labels, terminal blocks) and **Geist** (body text).

There is no package manager, no compile step, and no test suite. Open any `.html` file directly in a browser to preview.

## Site Structure

```
/                        ← root (main portfolio page)
  index.html             ← 404 error page (served by GitHub Pages as 404)
  style.css              ← shared design tokens and all reusable components
  DESIGN.md              ← design system reference (read before adding UI)
  portfolio/index.html   ← main portfolio: Hero, Skills, Pages, Contact
  vrc/index.html         ← VRChat avatar sub-page
```

The main portfolio lives at `portfolio/index.html`, not the root. The root `index.html` is the 404 page. Sub-pages reference the shared stylesheet as `../style.css`.

## Design System

Read `DESIGN.md` before making any UI changes — it is the authoritative source. Key rules:

- **Tokens**: all colors, radii, spacing, and fonts are CSS custom properties defined in `style.css :root`. Never use raw hex values or hardcoded px for things covered by tokens.
- **Border-radius rule**: cards `12px` (`--r-card`), buttons `6px` (`--r-btn`), tags `4px` (`--r-tag`). Never mix.
- **Typography**: monospace (`--mono`) for headings, labels, and terminal blocks; sans-serif (`--sans`) for body text. No Inter. No serif.
- **Glass card pattern**: `background: var(--glass-bg)` + `border: 1px solid var(--glass-border)` + `backdrop-filter: blur(18px)` + top-highlight `inset` shadow.
- **Nav links**: in-page anchors (`#section`) by default. Cross-page links only when explicitly required. Sub-pages that are not part of the main scroll flow omit the nav entirely and provide a self-contained back link.
- **Motion**: CSS transitions only — no JS animation libraries. Respect `prefers-reduced-motion`.

## Page Independence

Each page is a standalone document. Pages share `style.css` and the Google Fonts `<link>` tag but do not share any other infrastructure. Never assume a user arrived from another page in this site.

## Deployment

Hosted on GitHub Pages (`akvtr.github.io`). Pushing to `main` deploys automatically. No CI pipeline.
