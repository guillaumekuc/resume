**Live Resume**: https://guillaumekuc.github.io/resume

## About

Source of truth for my professional resume. Structured data source built on the [JSON Resume standard](https://jsonresume.org/). Content/Presentation decoupling. Fully customizable. Easy updates and version control through Git-based workflows. Full Data ownership and portability. 

## Setup
`npm install`

### Common Commands

- Build static HTML into `/docs` (published site): `npm run build`
- Export a PDF: `npm run pdf` (needs puppeteer's bundled Chromium, installed by `npm install`)

Rendering goes through [`resumed`](https://www.npmjs.com/package/resumed). The old
`resume-cli` (`classy-alt` theme, `npm run dev`) no longer works, and `resumed` has no
`serve` command, so re-run `npm run build` and refresh to preview changes.

### Theme

The theme lives in [`theme/`](theme/): a local fork of
[`jsonresume-theme-even`](https://www.npmjs.com/package/jsonresume-theme-even) 0.26.1,
installed as the `jsonresume-theme-local` package (`file:theme`). The only change from
upstream is that the education section renders the `score` field as `Grade: ...`
(upstream ignores it; `N/A`/empty scores are hidden). Edit `theme/index.js` to customise
the output.

## Maintenance Guide

- Edit `resume.json` using the [JSON Resume Schema](https://jsonresume.org/schema/)
- Re-render: `npm run build`
- Commit changes (including the regenerated `docs/index.html`)

### PDF export

`npm run pdf` renders via headless Chrome and comes out on a light background with no
extra flags needed: the theme ships `pdfRenderOptions = { mediaType: "print",
printBackground: true }`, which `resumed export` applies automatically, and the theme
only switches to its dark palette under `prefers-color-scheme: dark`, which headless
Chrome doesn't request by default. `resume.pdf` is gitignored (regenerate locally rather
than committing it).

### Known theme limitation: `publications[].highlights` is not rendered
The JSON Resume schema only defines `highlights` for `work`, `volunteer`, and
`projects` entries — not for `publications` (there it's just `name`,
`publisher`, `releaseDate`, `url`, `summary`). `jsonresume-theme-even` (and
`jsonresume-theme-classy`) follow that schema literally: their publications
template only reads those five fields, so any `highlights` array added to a
`publications` entry in `resume.json` is silently dropped when rendering —
it's not a bug in this repo's setup, just an unsupported field for that
section in both themes. If you want those bullet points to actually show up,
fold them into the `summary` string instead (as prose, comma/semicolon
separated, or with line breaks) rather than a `highlights` array.

Themes referenced in this repo:
- https://www.npmjs.com/package/jsonresume-theme-even (upstream of the local theme)
