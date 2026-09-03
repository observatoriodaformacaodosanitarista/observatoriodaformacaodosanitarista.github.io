# Contributing to the PPGCosmo Website

This is a [Quarto](https://quarto.org) website rendered to static HTML and deployed automatically to [ppgcosmo.github.io](https://ppgcosmo.github.io) on every push to `main`.

## Repository structure

```
ppgcosmo-website/
├── _quarto.yml               # Site config: navbar, footer, theme
├── styles.css                # Custom CSS overrides
├── index.qmd                 # Home page
├── people.qmd                # People page
├── applications.qmd          # Applications page
├── _data/                    # JSON data files — the main place to edit content
│   ├── news.json             #   News/highlights feed shown on the home page
│   ├── faculty_brazil.json   #   Faculty members based in Brazil
│   ├── collaborators.json    #   International collaborators
│   ├── phd_students.json     #   Current PhD students
│   ├── postdocs.json         #   Current postdoctoral researchers
│   ├── postdocs_previous.json#   Previous postdocs
│   ├── alumni.json           #   Alumni
│   ├── institutions.json     #   Partner institutions
│   └── organization.json     #   Coordinator and deputy coordinator
├── images/
│   ├── people/               #   Profile photos (see naming convention below)
│   └── ...                   #   Logos and banner images
└── .github/workflows/
    └── publish.yml           # CI/CD: renders and deploys to GitHub Pages
```

The `.qmd` pages read from the `_data/` JSON files using embedded Python. **In most cases, only the JSON files need to be edited** — no Python or Quarto knowledge required.

---

## Common edits

### Adding a news item

Edit `_data/news.json`. Insert the new entry at the top of the array. There are **two kinds of entries**.

**1. Person achievement** — highlights something done by a member of the program:

```json
{
  "date": "2026-05-15",
  "people": [{"name": "Full Name", "role": "fac"}],
  "text": "One or two sentences describing the achievement or event.",
  "urls": [{"label": "Link text", "url": "https://..."}]
}
```

**2. Call / announcement** — a program-wide notice (e.g. a fellowship call). Uses `type` and `title` instead of `people`:

```json
{
  "date": "2026-05-15",
  "type": "call",
  "title": "Short headline shown in bold",
  "text": "One or two sentences of detail (may be empty: \"\").",
  "urls": [{"label": "Link text", "url": "https://..."}]
}
```

| Field | Notes |
|---|---|
| `date` | ISO date (`"2026-05-15"`) or year-only (`"2026"`). Items are sorted newest-first; within the same date, order follows the JSON file. |
| `type` | Only on call/announcement entries: `call` or `announcement`. The value is shown as a colored tag next to the title. |
| `role` | Only on person entries: `fac` (faculty) · `pd` (postdoc) · `s` (PhD student) · `a` (alumnus/a) |
| `urls` | Can be `[]` (empty) or contain one or more `{"label": "...", "url": "..."}` objects. |

The home page feed shows only items from the current and previous calendar year.

---

### Adding or updating a person

**PhD students** — `_data/phd_students.json`:
```json
{"name": "Full Name", "photo": "images/people/firstname_lastname.jpg"}
```

**Faculty in Brazil** — `_data/faculty_brazil.json`:
```json
{"name": "Full Name", "affiliation": "Institution", "url": "https://...", "orcid": "0000-0000-0000-0000"}
```

**Current postdocs** — `_data/postdocs.json` (same structure as PhD students).

**Previous postdocs** — `_data/postdocs_previous.json`.

**Alumni** — `_data/alumni.json`.

**International collaborators** — `_data/collaborators.json`.

#### Profile photos

- Place the file in `images/people/` using the format `firstname_lastname.jpg` (lowercase, no accents, no spaces).
- Any common format works (jpg, jpeg, png). Photos are displayed as 90×90 px circles.

---

### Updating the applications page

Edit `applications.qmd` directly. It is plain Markdown with a short YAML header.

### Updating the coordination

Edit `_data/organization.json`.

---

## Local preview

Install [Quarto](https://quarto.org/docs/get-started/) and Python with Jupyter:

```bash
pip install jupyter
quarto preview      # live preview with auto-reload at localhost:4200
```

To do a full build without serving:

```bash
quarto render       # output goes to _site/
```

---

## Deployment

Pushing to `main` triggers the GitHub Actions workflow in `.github/workflows/publish.yml`, which renders the site and publishes it to GitHub Pages. No manual step is needed. All contributors who commit to the repository will appear in the [contributors graph](https://github.com/ppgcosmo/ppgcosmo-website/graphs/contributors).
