# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains two tightly coupled components for the **EchoStyle** research project (ECCV 2026, submission ID 6131):

1. **Project page** — a static GitHub Pages site showcasing the paper (based on [Academic Project Page Template](https://github.com/eliahuhorwitz/Academic-project-page-template))
2. **LaTeX paper** — the ECCV 2026 submission source in `paper/latex/`

EchoStyle is a text-driven video stylization framework built on Wan2.2-I2V-14B. Key contributions: a reverse-synthesis data pipeline (V-Style20k dataset, 20k video pairs), a multi-visual-condition alignment V2V architecture, and an init-follow-mode mechanism for long-video extension.

## Development

### Project Page (static site)

No build step, package manager, or bundler. Serve locally:
```bash
python -m http.server 8000
# or
npx serve .
```

No tests or linting configured.

### LaTeX Paper

```bash
cd paper/latex
pdflatex main
bibtex main
pdflatex main
pdflatex main
```

Supplementary material compiles separately:
```bash
pdflatex sup
```

**Review vs. camera-ready toggle**: controlled by commenting/uncommenting `\usepackage` lines at the top of `main.tex` and `sup.tex` (look for `TODO REVIEW` / `TODO FINAL` comments). Currently `main.tex` is set to camera-ready mode; `sup.tex` is in review mode (ID 6131).

## Architecture

### Project Page

- `index.html` — single-page site organized as HTML building blocks (hero, abstract, image carousel, video carousel, YouTube embed, PDF poster, BibTeX). Each section can be commented out as needed. Customization points are marked with `<!-- TODO: ... -->` comments.
- `static/css/index.css` — custom styles with CSS variables (`:root`) on top of Bulma
- `static/js/index.js` — "More Works" dropdown, BibTeX copy, scroll-to-top, video autoplay via IntersectionObserver, bulma-carousel/slider init
- `static/css/bulma.min.css` and other vendored libs — checked into `static/`, not fetched via npm or CDN
- `.nojekyll` — disables Jekyll processing on GitHub Pages

### LaTeX Paper

- `main.tex` — full paper (intro, related work, method §3.1–§3.3, experiments, conclusion)
- `sup.tex` — supplementary material (ablations, evaluation prompts, extra visual results)
- `main.bib` — bibliography with short-form conference macros (`@String(CVPR = {CVPR})` etc.)
- `llncs.cls`, `eccv.sty`, `eccvabbrv.sty`, `splncs04.bst` — ECCV/Springer template files (do not modify)
- `pdf/` — main paper figures (teaser, pipeline, dataset, comparisons, ablations)
- `sup_pdf/` — supplementary figures

**Author-defined commands** in both `main.tex` and `sup.tex`:
- `\fixme{text}` — cyan inline note (wenhan)
- `\hq{text}` — red inline note (Huaqiu)

## Key Conventions

- **CSS theming**: all colors/shadows/radii/transitions in `static/css/index.css` `:root` variables. Change theme there.
- **Performance loading**: critical CSS loads synchronously; non-critical CSS via `<link rel="preload" ... onload>` with `<noscript>` fallbacks. All JS is deferred.
- **SEO/social meta**: `<head>` contains Open Graph, Twitter Card, Google Scholar (`citation_*`), and JSON-LD (`ScholarlyArticle`). All must be updated together when changing paper metadata.
- **LaTeX figures**: referenced as `pdf/*.pdf` (vector PDFs), not raster images. When adding figures, place PDFs in `paper/latex/pdf/` or `paper/latex/sup_pdf/`.
- **Bilingual comments**: LaTeX source contains extensive Chinese comments alongside English documenting design rationale and ablation reasoning. These are authorial notes, not compilation artifacts.
