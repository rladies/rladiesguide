---
title: How the site is built
weight: 1
menuTitle: How the site is built
---

The RLadies+ global website is a Hugo site.
Not a blogdown site, not a Quarto site — a plain Hugo site that uses a custom theme called `hugo-rladiesplus`, vendored under [`themes/hugo-rladiesplus/`](https://github.com/rladies/rladies.github.io/tree/main/themes/hugo-rladiesplus).
If you have worked on a Hugo site before, the layout will feel familiar.
If you have not, the rest of this page will give you the mental model you need before you start poking at templates.

The theme started life as a fork of [Hugo Initio](https://miguelsimoni.github.io/hugo-initio-site/) and has since been rewritten on top of [Tailwind CSS v4](https://tailwindcss.com) and [Alpine.js](https://alpinejs.dev).

## What you actually need on your machine

To clone, edit, and preview the site you need exactly two things: [Hugo Extended](https://gohugo.io/installation/) ≥ 0.144 and Git.
You do **not** need Node, npm, R, or renv to run `hugo server` — the theme ships with its CSS and JavaScript pre-built.

```bash
git clone https://github.com/rladies/rladies.github.io.git
cd rladies.github.io
hugo server
```

The site builds in a couple of seconds and opens at <http://localhost:1313>.

The R toolchain (`renv`, the scripts under `scripts/`) is only needed when you are working on the data pipeline locally — fetching the directory, generating placeholder translations, validating JSON.
For everyday work on content and templates, Hugo on its own is enough.

## The shape of the repository

```
rladies.github.io/
├── archetypes/        # not used; the theme owns the archetypes
├── assets/            # site-level assets (mostly directory member photos)
├── config/            # split config: _default, development, production
├── content/           # all the actual pages, in markdown
├── data/              # JSON/YAML used by templates (chapters, directory, etc.)
├── i18n/              # translation strings: en.yaml, es.yaml, pt.yaml, fr.yaml
├── layouts/           # site-level layout overrides (rare; the theme owns most)
├── scripts/           # R scripts used by GitHub Actions, not by Hugo
├── static/            # files served verbatim at the site root
├── themes/
│   └── hugo-rladiesplus/   # the theme — most templates live here
└── .github/workflows/      # CI: build, preview, lint, lighthouse, etc.
```

The single most important habit is: look in the theme first, and the site second.
When you wonder where the chapters page is rendered, the answer is [`themes/hugo-rladiesplus/layouts/chapters/list.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/chapters/list.html), not the site root.
When you wonder where the directory card markup lives, it is in the theme's partials, not the site's.

## Configuration

Most Hugo projects have a single `config.toml` or `hugo.yaml` at the root.
This site uses a [config directory](https://gohugo.io/getting-started/configuration/#configuration-directory) instead, so settings can be split by environment and by purpose.

```
config/
├── _default/
│   ├── hugo.yaml        # core Hugo settings
│   ├── languages.yaml   # the four supported languages
│   ├── markup.yaml      # goldmark settings
│   ├── menu.yaml        # main navigation
│   └── params.yaml      # site-wide params used by templates
├── development/
│   └── hugo.yaml        # local dev: relative baseURL, drafts on
└── production/
    └── hugo.yaml        # production: rladies.org baseURL, only English
```

Hugo merges `_default/` with the active environment.
Locally, `hugo server` uses `development/`.
The production GitHub Action runs `hugo -e production`, which switches the base URL to <https://rladies.org/> and disables `es`, `pt`, and `fr` until those translations are reviewed.

The split also keeps the menu config readable.
The English menu lives in [`menu.yaml`](https://github.com/rladies/rladies.github.io/blob/main/config/_default/menu.yaml), and language-specific menu overrides (currently French) live alongside their language entry in [`languages.yaml`](https://github.com/rladies/rladies.github.io/blob/main/config/_default/languages.yaml).
A new menu item only appears once both the menu config and the matching `i18n/<lang>.yaml` translation key exist — Hugo emits the link, but the human-readable label comes from the i18n file via the `T .Identifier` template call.

## How content is organised

Hugo's content tree maps directly onto the URL tree.
A page lives at `content/<section>/<slug>/index.<lang>.md`, and Hugo serves it at `/<section>/<slug>/`.

Every page is a [page bundle](https://gohugo.io/content-management/page-bundles/) — a folder named after the slug, containing an `index.md` plus any images or other files the page needs.
Bundling matters because it lets the markdown reference its images with simple relative paths (`![](rladies-bioc.png)`) and lets Hugo's image processing pipeline find, resize, and fingerprint them automatically.

The site does not use `.Rmd` or `.qmd` as content sources.
Blog posts are plain `.md`.
If a contributor authors a post in Quarto, they commit the rendered `.md` next to the `.qmd` and Hugo only ever sees the plain markdown.
The corresponding R Markdown / Quarto files are excluded from the build via `ignoreFiles` in `config/_default/hugo.yaml`.

Multilingual pages use Hugo's [translation by filename](https://gohugo.io/content-management/multilingual/#translation-by-filename) convention.
The English copy of a post is `index.en.md`, the Spanish copy is `index.es.md`, and so on.
Section index pages follow the same rule: `_index.en.md`, `_index.es.md`.

## Sections and their layouts

Hugo picks a layout based on the [section](https://gohugo.io/content-management/sections/) a page lives in.
The theme defines a custom layout for each section that renders something more than plain prose.

| Section                    | Layout used                                    | What it does                                                  |
| -------------------------- | ---------------------------------------------- | ------------------------------------------------------------- |
| `/` (home)                 | `_default/index.html`                          | Hero + impact + chapters map + involved + featured + sponsors |
| `/blog/`                   | `post/list.html`, `post/single.html`           | Blog listing and post page (with TOC, related, prev/next)     |
| `/news/`                   | `post/list.html`, `post/single.html`           | Same as blog — news posts share the post layout               |
| `/chapters/`               | `chapters/list.html`, `chapters/single.html`   | World map + filterable table; per-chapter page                |
| `/directory/`              | `directory/list.html`, `directory/single.html` | Filterable member grid; per-member profile                    |
| `/events/`                 | `events/list.html`                             | FullCalendar view of upcoming events                          |
| `/about-us/global-team/`   | `global-team/list.html`                        | Leadership / team table / alumni grid                         |
| `/about-us/our-story/`     | `our-story/list.html`                          | Story sections + photo lightbox                               |
| `/about-us/involved/`      | `involved/list.html`                           | Pillars + program rows + GitHub `help-wanted` issues feed     |
| `/about-us/faq/`           | `faq/list.html`                                | Filterable FAQ accordion                                      |
| `/about-us/press/`         | `press/list.html`                              | Filterable press list with optional PDF                       |
| `/programs/`               | `programs/list.html`                           | Alternating program rows with CTAs                            |
| `/form/...` (any redirect) | `redirect/single.html`                         | Pretty-URL page that meta-refreshes elsewhere                 |
| Anything else              | `_default/list.html`, `_default/single.html`   | Generic listing and single-page templates                     |

The vacancies, packages, and online-content layouts handle smaller features (open roles on the global team, the package hex wall, the curated awesome-content grid).

If you are adding a brand-new kind of page that needs unique markup — say, an interactive timeline — you have two honest choices.
You can write the new layout into the theme as `<section>/list.html` and use it from a new content section.
Or, if it is a one-off, you can author the page in markdown and embed the custom HTML inline.
Pick the option that matches how often the page will change and who will maintain it.

## Render hooks: how markdown becomes HTML

Goldmark, Hugo's markdown renderer, lets you customise how specific markdown elements turn into HTML via [render hooks](https://gohugo.io/render-hooks/introduction/).
The theme uses three.

[`_markup/render-image.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/_markup/render-image.html) turns block images into `<figure>` with a caption when the markdown image has a title; inline images stay as `<img>`.
All images get `loading="lazy"` automatically.

[`_markup/render-link.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/_markup/render-link.html) gives any link starting with `http` a `target="_blank" rel="noopener noreferrer"`.
Internal links are unchanged.
You do not need to write the target attributes by hand.

[`_markup/render-codeblock-mermaid.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/_markup/render-codeblock-mermaid.html) wraps a fenced code block tagged ` ```mermaid ` in a `<pre class="mermaid">` and triggers the mermaid runtime to load on that page only.
Pages without diagrams do not pay for the mermaid bundle.

This is the reason the site can avoid HTML in markdown almost entirely.
Write `![alt](image.png "Caption text")` and you get a captioned figure.
Write a fenced mermaid block and you get a rendered diagram.

## Shortcodes

Beyond the [standard Hugo shortcodes](https://gohugo.io/content-management/shortcodes/), the theme provides three custom ones.

`{{</* button "https://example.com" "Button label" */>}}` renders a branded primary button.
It also accepts named parameters: `{{</* button url="..." text="..." */>}}`.

`{{</* callout type="tip" title="Optional title" */>}}` renders a boxed callout.
Valid types are `tip`, `info`, `warning`, `danger`, `note`.
The body uses normal markdown.

`{{</* rlogo */>}}` inlines the RLadies+ submark.
It is mostly useful for hero panels and decorative SVG logos.
Accepts `width`, `class`, `color`, `alt`, and an optional `image` argument that fills the logo with a portrait.

Use shortcodes when you find yourself reaching for raw HTML.
The shortcodes give you the brand styling for free, and they keep markdown readable.

## Hugo Pipes and the asset pipeline

The theme bundles its own CSS and JavaScript using [Hugo Pipes](https://gohugo.io/hugo-pipes/introduction/).
At build time, Hugo concatenates the relevant assets, fingerprints them with a content hash, minifies them, and writes integrity hashes into the `<link>` and `<script>` tags.
That gives the site cache-bustable URLs and subresource integrity protection without any extra tooling on the build server.

The pre-built artefacts the theme ships with — Tailwind's compiled CSS, the FullCalendar bundle, the d3-geo map bundle, FontAwesome SCSS, the Alpine and Mermaid runtimes — live under `themes/hugo-rladiesplus/assets/{css,js,scss}/vendor/`.
They are committed.
This is what lets a clone build with only Hugo installed.

The npm side is described in detail under [Theme assets and npm bundling](/website/admin_guide/assets/).
The short version: contributors who edit the theme's `assets/css/main.css` or update a vendor library run `npm install` inside the theme to regenerate the vendor bundles, then commit the result.
Everyone else benefits without ever touching Node.

## Data, and what it is for

Hugo loads anything under `data/` as a structured value templates can read.
The site uses this for several purposes.

`data/chapters/` holds one JSON file per chapter — the source of truth for chapter pages and the world map.

`data/directory/` holds one JSON file per directory member, populated from the [rladies/directory](https://github.com/rladies/directory) repo at build time.
The website repo only keeps `data/directory/sample-*.json` for local previews.

`data/global_team/` holds leadership, team, alumni, and vacancies, populated from Airtable by the [`global-team.yml`](https://github.com/rladies/rladies.github.io/blob/main/.github/workflows/global-team.yml) GitHub Action.

`data/continents.yaml` maps countries to continents and is used to group the chapter and directory filters.

`data/months/<lang>.yaml` localises month names so dates read naturally in every supported language.

`data/read_in_lang.yaml` provides the "Read in &lt;language&gt;" label for the post-translation badge.

`data/sponsors.json` drives the sponsor list rendered on the home page.

`data/stock_images.json` is the catalogue of stock images used by the programs, our-story, and involved sections, with attribution.

Some pages also pull data live, at build time, from public GitHub raw URLs:

- `https://raw.githubusercontent.com/rladies/meetup_archive/refs/heads/main/data/events.json` — every chapter's upcoming events, used by the `/events/` calendar.  
- `https://raw.githubusercontent.com/rladies/awesome-rladies-creations/main/data/website/awesome_packages.json` — the package hex wall and per-member package lists.  
- `https://raw.githubusercontent.com/rladies/awesome-rladies-creations/main/data/website/awesome_content.json` — the curated podcasts/blogs/newsletters grid.  

These remote fetches go through `resources.GetRemote` so Hugo caches them locally — your `hugo server` only hits the network on the first build.

The directory deserves its own page because it threads through so much of the site.
That story is in [The directory: where members live and how the site uses them](/website/admin_guide/directory-pipeline/).

## Static

`static/` contains files served at the site root with no processing.
Logos, the favicon, the hero video, the Open Graph default image, the FontAwesome and Poppins woff2 files all live here.

The general rule: if a file is specifically used by one page, bundle it into that page's folder.
If it is used by many pages or by the chrome of the site (logos, favicons), put it in `static/` and reference it with a leading slash.

```text
/images/logo.svg   → static/images/logo.svg, served at https://rladies.org/images/logo.svg
images/logo.png    → relative to the current page bundle
```

## What the homepage actually does

The homepage is composed of a stack of partials, in this order: impact stats, chapters strip with the world map, get-involved pillars, featured members, latest blog and news, sponsors.
Each partial is independent.
If you want to remove a section, comment out one line in [`_default/index.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/_default/index.html).
If you want to change the order, move the lines.

The data each partial needs is computed once in [`partials/head/data.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/partials/head/data.html) and stashed in `.Store` so every later partial can read it without doing the work again.
This is where the merged chapter list, the active-chapter list, and the live events feed are populated.

## Dark mode

Dark mode is opt-in via a toggle in the navigation, and respects `prefers-color-scheme` until the user expresses a preference.
The implementation is in [`assets/js/darkmode.js`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/assets/js/darkmode.js) and is loaded inline in `<head>` to avoid the flash-of-wrong-theme.
Component CSS uses the `dark:` Tailwind variant against a `.dark` class on `<html>`.
There is no separate dark stylesheet to maintain.
If a colour does not yet have a dark counterpart, add it under [`components/darkmode.css`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/assets/css/components/darkmode.css) — the rest of the components are unchanged.

## Translations and i18n

Three different things on this site can be translated, and they live in three different places.
Static UI strings — "Read more", "Skip to main content", "Find a Chapter" — live in [`i18n/<lang>.yaml`](https://github.com/rladies/rladies.github.io/tree/main/i18n) and are pulled into templates with `{{ i18n "key" }}`.
Page content — the prose of an article, the body of an FAQ — lives next to the English content as `index.<lang>.md`.
Localised data — month names, the "Read in" label — lives under `data/months/` and `data/read_in_lang.yaml`.

When a content page exists in English but not in another language, the [`scripts/missing_translations.R`](https://github.com/rladies/rladies.github.io/blob/main/scripts/missing_translations.R) script (run by every build job) creates a placeholder copy with `translated: auto` in its front matter.
The site renders that placeholder under the foreign-language URL with a banner explaining the page is auto-translated and inviting review.
That is how the site can claim to support four languages without having reviewed translations for every one of its hundreds of pages.

The full lifecycle — adding a language, reviewing translations, what `translated: auto` versus `translated: false` means — is covered under [Multi-lingual support](/website/mulit-lingual/).
