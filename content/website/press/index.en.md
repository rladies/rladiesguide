---
title: "Adding a press mention"
menuTitle: "Add press entry"
weight: 6
---

The press page at <https://rladies.org/about-us/press/> lists every press mention RLadies+ has earned, year by year.
Each entry is a small page bundle under `content/about-us/press/<date-slug>/`.
Adding one is a small PR — a folder, an `index.en.md`, optionally a PDF.

## Folder structure

Each press mention is its own folder named after the publication date in `YYYY-MM-DD` form.
If two entries land on the same date, append the outlet name to the folder:

```
content/about-us/press/2021-10-30-times/
└── index.en.md
content/about-us/press/2021-10-30-buzzfeed/
└── index.en.md
content/about-us/press/2021-12-23/
└── index.en.md
```

The folder name does not need to be repeated in the URL — Hugo will use it as the slug, so the article ends up at `/about-us/press/2021-10-30-times/`.

## File content

The press layout reads only the front matter; the body of the file can stay empty.
A complete entry:

```yaml
---
title: "El aporte silencioso a la agroinformática"
date: "2017-06-26"
source: "http://ria.inta.gob.ar/contenido/el-aporte-silencioso-la-agroinformatica"
language: "es"
---
```

Fields:

`title` — the article title in its original language. The press list shows the title verbatim with a `lang` attribute set, so screen readers can pronounce it correctly.

`date` — the publication date as `YYYY-MM-DD`.

`source` — the full URL to the article.

`language` — the ISO 639-1 code for the article's language (`en`, `es`, `pt`, `fr`, `de`, …). This is what powers the `lang` attribute on the title and helps the press list categorise content. Optional but recommended.

## Optional: a local PDF

If the article is paywalled or might disappear, we can host a PDF copy alongside the front matter:

```
content/about-us/press/2021-10-30-times/
├── index.en.md
└── 2021-10-30-times-when-r-ladies-came-to-paris.pdf
```

Any `.pdf` file in the bundle becomes a "Download PDF" button on the press list.
Use a descriptive filename — the visitor downloads exactly that name.

Confirm with the article's publisher that hosting the PDF is acceptable, particularly if the article is behind a paywall.

## Submitting the entry

[Working with the website](/website/fork-clone-pr/) covers the general clone-and-PR workflow.
The press entry is small enough that the GitHub web UI works fine — create a new file at the right path, fill in the front matter, write a commit message, propose the change.

A team member from `@rladies/website` will review and merge.
