# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A **Quarto website** for the World Bank GeoPov team (geospatial poverty analysis), published to GitHub Pages at [worldbank.github.io/geoPovLearn](https://worldbank.github.io/geoPovLearn/). There is no application code, build toolchain, or test suite — content is authored in `.qmd` (Quarto markdown) and rendered by the `quarto` CLI. Most "development" is writing/editing `.qmd` files and tuning theming.

## Common commands

```bash
quarto preview                              # live-reloading preview of the whole site
quarto render                               # render entire site to _site/
quarto preview seminars/seminar-2026-02/slides.qmd   # preview a single doc (e.g. a slide deck)
quarto render seminars/seminar-2026-02/slides.qmd    # render a single doc
quarto check                                # diagnose the Quarto/R/Python install
```

There are no lint or test commands. Verify changes by rendering and viewing the output.

## Deployment

Push to `main` triggers `.github/workflows/publish.yml`: GitHub Actions runs `quarto render` and publishes the built site to the `gh-pages` branch. **Do not edit `gh-pages` or `_site/` by hand** — both are generated. The live site only updates on push to `main`.

Branch note: day-to-day work happens on `ebdev`; `main` is the publish branch. `old-page`/`bbdev` are legacy.

## Architecture

- **`_quarto.yml`** is the single source of truth for site structure: navbar, the docked sidebar, and which `.qmd` files appear where. Adding a page means adding both the `.qmd` file *and* a sidebar entry here. Site-wide format is the `cosmo` theme + `styles.css`.
- **`execute: freeze: auto`** — computational cells are only re-executed when their source changes (frozen results live in `_freeze/`). A reader/CI machine without R/Python can still render unchanged content.
- **Content directories**, each a sidebar section: `index.qmd` (About), `seminars/`, `projects/`, `team/`. Team pages follow a per-person `name.qmd` + `name-photo.jpg` pattern wired up in `_quarto.yml`.
- **Seminars** (`seminars/seminar-YYYY-MM/`, `course-YYYY-MM/`) each bundle `overview.qmd`, `setup-instructions.qmd`, `qa.qmd`, and a reveal.js deck `slides.qmd`. Slide decks are self-contained: they have their own `custom.scss`/`custom.css`, a `title-slide.html` template partial, and embedded animation iframes served from sibling asset folders (`grid-worms/`, `boids/`, `logo-fall/`, `images/`) declared under `format.revealjs.resources`.
- **`_extensions/verticator/`** is a vendored reveal.js extension (progress indicator) — extension code, not site content.
- Branding assets live in `assets/` (WBG/Prosperity logos, favicon). Licensing is MIT + the World Bank IGO Rider (`WB-IGO-RIDER.md`); keep both when touching `LICENSE`.

## Skills (`.claude/skills/`)

Project skills are vendored under **`.claude/skills/`** and pinned in **`skills-lock.json`** (sourced from the `posit-dev/skills` GitHub repo — treat them as managed/read-only). The ones most relevant to work here:

- **`quarto-authoring`** — authoring `.qmd`: callouts, cross-references, citations, layout, diagrams, code cells, YAML front matter. Reach for this for almost any content edit.
- **`alt-text`** — generating/auditing accessible `fig-alt` for figures and images.
- **`brand-yml`** / **`shiny-bslib-theming`** — theming and brand colors (also see the `geopovbrand-quarto` skill for this repo's `_brand.yml`/SCSS/sidebar patterns).
- **`pr-create`**, **`pr-threads-address`**, **`pr-threads-resolve`** — GitHub PR workflow.

The remaining skills (`r-package-development`, `testing-r-packages`, `cli`, `mirai`, `ggsql`, etc.) target R-package development and are mostly inapplicable to this website repo.
