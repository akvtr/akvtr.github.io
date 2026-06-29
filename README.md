# akvtr.github.io

Personal portfolio site. Live at **https://akvtr.github.io/**

## Stack

Plain HTML + CSS — no frameworks, no build step, no package manager.  
Fonts: [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono) + [Geist](https://fonts.google.com/specimen/Geist) via Google Fonts.

## Structure

```
/
├── index.html          # 404 page (served by GitHub Pages on unknown routes)
├── style.css           # shared design tokens and all reusable components
├── DESIGN.md           # design system reference
├── portfolio/
│   └── index.html      # main portfolio (Hero, Skills, Pages, Contact)
└── vrc/
    └── index.html      # VRChat avatar sub-page
```

## Development

Open any `.html` file directly in a browser — no server or build step needed.

```bash
# Quick preview in default browser (macOS / Linux)
open portfolio/index.html

# Or with Python's built-in server for accurate relative-path resolution
python -m http.server 8080
```

## Deployment

Pushing to `main` deploys automatically via GitHub Pages. No CI pipeline.
