# Theory@UMass

Source for the [Theory@UMass](https://umass-tcs.github.io) club website. Built with [Eleventy](https://www.11ty.dev/), deployed to GitHub Pages via [.github/workflows/deploy.yml](.github/workflows/deploy.yml) on every push to `main`.

## Editing content

Nearly everything on the site is data, not markup. To update it, edit the relevant file in `src/_data/`:

| File | Controls |
|---|---|
| `home.json` | The weekly meeting and next talk shown on the homepage |
| `talks.json` | Upcoming talks and the talk archive |
| `officers.json` | The People page — add/remove/edit officers here (portraits go in `src/assets/`, omit `portrait` for a placeholder) |
| `courses.json` | The Courses page |
| `reading.json` | The Reading page (books and papers) |
| `problemSets.json` | The Resources page, grouped by term |
| `projects.json` | The Other page |
| `site.json` | Nav links, footer text, Instagram handle/URL |

PDFs go in `src/psets/` (problem sets) or `src/talks/` (slides), referenced by URL-encoded path from the relevant data file.

Page layout, fonts, and colors live in `src/_includes/base.njk` (page shell/header/footer), `src/_includes/macros.njk` (reusable row/section components), and `src/styles/main.css` (all styling — design tokens are CSS custom properties at the top of the file).

## Local development

```
npm install
npm run serve   # build + serve at http://localhost:8080 with live reload
npm run build   # build to _site/
```

## Deployment

Pushing to `main` triggers `.github/workflows/deploy.yml`, which builds the site and publishes `_site/` via GitHub Pages. This requires the repo's **Settings → Pages → Build and deployment** source to be set to **GitHub Actions**.
