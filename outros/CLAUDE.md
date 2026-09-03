# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Static website for PPGCosmo (International PhD Program in Astrophysics, Cosmology and Gravitation at Ufes, Brazil), built with [Quarto](https://quarto.org) and deployed to GitHub Pages at [ppgcosmo.org](https://ppgcosmo.org).

## Commands

```bash
pip install jupyter        # one-time: Python + Jupyter are required to execute embedded code
quarto preview             # live preview with auto-reload
quarto render              # full build → _site/
```

There is no test suite or linter. "Correctness" means the site renders without errors and the pages look right in preview.

## Deployment

Pushing to `main` triggers `.github/workflows/publish.yml`, which runs `quarto render` (Quarto **pinned to 1.7.32**) and publishes `_site/` to GitHub Pages. No manual deploy step. If you change how pages render, verify with `quarto render` locally before pushing, since a render failure breaks the live site.

## Architecture

The site is **data-driven**: `.qmd` pages contain embedded Python (executed at render time via Jupyter) that reads JSON from `_data/` and emits HTML. Editing content almost always means editing a `_data/*.json` file, not the pages.

- **Pages**: `index.qmd` (home + news + coursework), `people.qmd`, `applications.qmd` (plain Markdown, edit directly), `contributors.qmd`.
- **`_data/*.json`**: the content source of truth — people lists (`faculty_brazil`, `phd_students`, `postdocs`, `postdocs_previous`, `alumni`, `collaborators`), `institutions`, `organization` (coordinator/deputy), and `news.json`.
- **`_quarto.yml`**: site-wide config (navbar, footer, `flatly` theme, `page-layout: full`). `styles.css` holds CSS overrides.
- **`contributors.qmd`** builds its table by shelling out to `git log --numstat` at render time and maps author names → GitHub handles via `_data/github_usernames.json`. Add new committers there to get them linked.

### news.json entries have two shapes

`index.qmd`'s `news_feed()` renders two kinds of entries (see `TYPE_COLORS` / `ROLE_LABELS` in the file):

1. **Call / announcement** — has `"type": "call"` (or `"announcement"`) and a `"title"`. Rendered with a colored type tag.
2. **Person achievement** — has a `"people"` array of `{"name", "role"}` where role is `fac|pd|s|a`. No `type`/`title`.

Both use `date` (ISO `"2026-05-15"` or year-only `"2026"`), `text`, and `urls` (possibly `[]`). Entries are sorted newest-first; the home feed shows only the current and previous calendar year (`years=2` cutoff). Note: `CONTRIBUTING.md` documents only the person shape — the call/announcement shape is real and in active use.

### Profile photos

`images/people/firstname_lastname.jpg` (lowercase, no accents/spaces), referenced by the `photo` field in the people JSON. Displayed as 90×90 px circles.

## Notes

- `*.quarto_ipynb` files are Quarto's intermediate render artifacts — gitignored, safe to ignore; `.quarto/` and `_site/` likewise.
- `CONTRIBUTING.md` is the human-facing editing guide and covers the common content edits in more detail.
