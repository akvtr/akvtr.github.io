# DESIGN.md

Design reference for this portfolio site. All pages share `style.css` and follow these rules.

---

## Stack

- Plain HTML + CSS (no frameworks, no build step)
- Fonts via Google Fonts: **JetBrains Mono** (headings, labels, terminal) + **Geist** (body)
- No JavaScript unless explicitly noted

---

## Tokens (`style.css` `:root`)

| Token | Value | Usage |
|---|---|---|
| `--bg` | `#060814` | Page background base |
| `--accent` | `#b57bff` | Primary accent — purple, used everywhere consistently |
| `--accent-subtle` | `rgba(181,123,255,0.10)` | Tinted fills |
| `--accent-border` | `rgba(181,123,255,0.22)` | Elevated card borders |
| `--glass-bg` | `rgba(255,255,255,0.035)` | Glass card fill |
| `--glass-border` | `rgba(255,255,255,0.07)` | Glass card border / dividers |
| `--text-1` | `#ede8ff` | Primary text |
| `--text-2` | `rgba(237,232,255,0.52)` | Secondary / body text |
| `--text-3` | `rgba(237,232,255,0.28)` | Muted text, footer, labels |
| `--mono` | `'JetBrains Mono', monospace` | Headings, labels, terminal blocks |
| `--sans` | `'Geist', system-ui, sans-serif` | Body paragraphs |
| `--r-card` | `12px` | Card border-radius |
| `--r-btn` | `6px` | Button border-radius |
| `--r-tag` | `4px` | Tag / badge border-radius |
| `--nav-h` | `64px` | Fixed nav height |
| `--page-max` | `1280px` | Max content width |
| `--px` | `clamp(1.5rem, 6vw, 5rem)` | Horizontal page padding |

Corner-radius rule: **cards `12px`, buttons `6px`, tags `4px`** — never mix.

---

## Background

```css
background-color: var(--bg);
background-image:
  radial-gradient(ellipse 80% 55% at 12% -8%,  rgba(88,28,135,0.26) 0%, transparent 58%),
  radial-gradient(ellipse 60% 45% at 88% 108%, rgba(28,18,80,0.32)  0%, transparent 55%);
```

Deep navy base with two soft purple radial glows: top-left and bottom-right.

---

## Typography

- **Section headings / hero headline**: `font-family: var(--mono)`, `font-weight: 700`, `letter-spacing: -0.025em`
- **Body text**: `font-family: var(--sans)`, `font-weight: 300`, `line-height: 1.65`
- **Labels / eyebrows**: `font-family: var(--mono)`, `font-size: 0.6875rem`, `letter-spacing: 0.12em`, `text-transform: uppercase`, `color: var(--accent)`
- No serif fonts.
- No Inter.

---

## Glass Cards

```css
background: var(--glass-bg);
border: 1px solid var(--glass-border);
border-radius: var(--r-card);
backdrop-filter: blur(18px);
-webkit-backdrop-filter: blur(18px);
box-shadow:
  inset 0 1px 0 rgba(255,255,255,0.055),
  0 28px 72px rgba(0,0,0,0.45);
```

Elevated variant (Pages card, contact card) uses `var(--accent-border)` instead of `var(--glass-border)` and adds a subtle purple gradient fill.

---

## Landing Hub (`/index.html`)

The root page is a compact **vertical list of glass cards** (`.hub-list` / `.hub-card`), centered in the viewport. No horizontal grid — cards stack in a single column, `max-width: 480px`.

Each card is two short lines:

```
[BADGE] Title          →
        Description
```

- **Layout**: `.hub-card-head` (badge + title inline) on the first line, `.hub-card-desc` below. Padding stays tight (`~0.95rem 1.1rem`) so each card is only as tall as its two lines.
- **Badge tag**: same bordered-tag pattern as `.page-badge` — mono, `var(--accent)`, `1px solid var(--accent-border)`, `border-radius: var(--r-tag)`. Used inline as a label, not a full-width eyebrow.
- **Right-side indicator**: a single glyph absolutely positioned, vertically centered at `right: 1.1rem`. Link cards use `→` (nudges right on hover); the Lab toggle uses `▾` (rotates 180° when expanded).
- **Lab accordion**: the `Lab` card is a `<button>` that expands a nested sub-card (`class-gen`) below it via a `grid-template-rows: 0fr → 1fr` transition. The sub-card is indented (`margin-left`) to show hierarchy. This replaces the former nav dropdown.

This page omits the nav entirely (see Nav omission).

---

## Nav

- Fixed, `height: var(--nav-h)` (64px), `backdrop-filter: blur(16px)`
- Logo left: `~/portfolio` in `var(--accent)`, mono font
- Links right: mono, `var(--text-2)` → `var(--text-1)` on hover

### Nav link rule

**Nav links are in-page anchors by default** (`#section`). Each page is self-contained and independent. Do not link to other pages from the nav unless explicitly specified. The pattern is:

```html
<!-- Same page -->
<a href="#skills">Skills</a>

<!-- Cross-page (only when explicitly required) -->
<a href="/other-page/#section">Label</a>
```

### Nav omission

Pages that are not part of the main scroll flow (e.g. error pages, standalone sub-pages) **omit the nav entirely**. A back link or CTA inside the page content is sufficient.

---

## Buttons

| Class | Background | Text color | Use |
|---|---|---|---|
| `.btn-primary` | `var(--accent)` | `#07040f` (near-black) | Primary CTA |
| `.btn-ghost` | transparent | `var(--text-2)` | Secondary CTA |
| `.btn-gh` | `rgba(255,255,255,0.07)` | `var(--text-1)` | GitHub / external link |

All buttons: `border-radius: var(--r-btn)`, mono font, `white-space: nowrap`.  
Active state: `transform: translateY(1px) scale(0.99)`.

No two buttons on the same page should share the same intent.

---

## Sections

Sections use `.section-inner` for max-width + padding:

```css
.section-inner {
  max-width: var(--page-max);
  margin: 0 auto;
  padding: clamp(4rem, 7vw, 5.5rem) var(--px);
}
```

Sections are separated by `.divider` (`border-top: 1px solid var(--glass-border)`).

Eyebrow labels (small uppercase text above a heading) are used sparingly: **max 1 per 3 sections**.

---

## Page Independence

Each page (`index.html`, `404/index.html`, sub-pages like `vrc/index.html`, etc.) is **a standalone document**. Pages:

- Share `style.css` via relative path (`../style.css` from subdirectories)
- Share Google Fonts `<link>` in `<head>`
- Do **not** assume the user arrived from another page in this site
- Include their own nav only when they are part of the main scroll flow
- Provide a self-contained way back (a home link or CTA) when nav is omitted

---

## Responsiveness

| Breakpoint | Behavior |
|---|---|
| `> 900px` | Hero: 2-col grid (text left, terminal right) |
| `≤ 900px` | Hero: 1-col stack |
| `≤ 768px` | Skills grid: 2-col; contact card padding reduced |
| `≤ 480px` | Skills grid: 1-col; nav links gap reduced |

---

## Motion

`MOTION_INTENSITY: 3` — CSS only, no JavaScript animation libraries.

- Hover: `transition` on `color`, `background`, `border-color`, `opacity`
- Active: `transform: translateY(1px) scale(0.99)`
- Terminal cursor: `animation: blink 1.1s step-end infinite`
- `prefers-reduced-motion`: cursor animation disabled, all transitions collapsed to `0.01ms`

---

## Page Index

| Path | Description | Nav |
|---|---|---|
| `/index.html` | Landing hub — compact vertical card list (also served as 404) | No |
| `/portfolio/index.html` | Main portfolio — Hero, Skills, Pages, Contact | Yes |
| `/vrc/index.html` | VRChat avatar works | No |
| `/lab/class-gen/index.html` | UML class diagram editor tool | No |
