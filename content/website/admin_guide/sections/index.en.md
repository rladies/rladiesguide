---
title: A tour of the sections
weight: 5
menuTitle: Section-by-section tour
---

The site has a lot of pages.
Most of them fall into a small number of patterns, and each pattern has its own template, its own data sources, and its own quirks.
This page is a guided tour: section by section, what it is, where its layout lives, what feeds it, and what to know before you change it.

## Home

Layout: [`_default/index.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/_default/index.html).
The homepage is composed of six partials, each rendering one strip:

1. **Impact stats** — `partials/home/impact.html`. Pulls counts from the chapter data and the live events feed.  
2. **Chapters strip** — `partials/home/chapters.html`. The world map (d3-geo) plus a CTA to browse chapters.  
3. **Get involved** — `partials/home/involved.html`. Three pillar cards.  
4. **Featured members** — `partials/home/featured.html`. A small selection from the directory.  
5. **Latest blog and news** — `partials/home/blog.html`. Pulls the most recent posts from `/blog/` and `/news/`.  
6. **Sponsors** — `partials/home/sponsors.html`. Reads `data/sponsors.json`.  

To remove or reorder a strip, edit `_default/index.html` directly.
There is no "homepage builder" — the order is the file.

## Blog and news

Layouts: `post/list.html` (the index pages) and `post/single.html` (a single post).
Both `/blog/` and `/news/` use these layouts because the front-matter shape is the same.
Each post is a page bundle under `content/blog/<year>/<slug>/index.<lang>.md` (or `content/news/...`).

Front matter for a post:

```yaml
---
title: "A clear, sentence-case title"
description: "One-line subtitle, used in OG meta and post listings"
author:
  - name: Author Name
    directory_id: "author-slug"   # optional; turns name into a profile link
date: "2026-04-15"
categories: [partnerships, community]
tags: [bioconductor, research]
image:
  path: "feature-image.png"
  alt: "Description of the feature image for screen readers"
summary: |
  Optional override for the listing summary.
---
```

The single-page layout shows a hero image, post metadata (date, reading time, authors, editorial credit, translators), categories and tags, the body, related posts, prev/next navigation, and an editorial sidebar.
A table of contents appears on the right when the rendered post has more than ~100 characters of TOC.
The blog layout adds a "Contribute" callout pointing at `blog@rladies.org`.

Two GitHub Actions enforce post quality:

- [`blog-lint.yaml`](https://github.com/rladies/rladies.github.io/blob/main/.github/workflows/blog-lint.yaml) — checks frontmatter for required fields, valid date format, structured author with `directory_id`, image alt text.  
- [`checklist-blogpost.yaml`](https://github.com/rladies/rladies.github.io/blob/main/.github/workflows/checklist-blogpost.yaml) — posts the editorial checklist as a PR comment.  

## Chapters

Layouts: [`chapters/list.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/chapters/list.html), [`chapters/single.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/chapters/single.html).
The list page renders three things in sequence: a d3-geo world map of every active chapter (with hover tooltips showing member counts), a country/city filter card, and a country-grouped table of every chapter linked to its detail page.
The single-chapter page shows organisers (current and former), upcoming events for that chapter (filtered from the global meetup feed), and social links.

Chapter data lives in `data/chapters/`, one JSON file per chapter, keyed by `<country-state-city>.json`.
The map source data is the live meetup feed — a country count comes from the chapter JSON, but the dot positions are the live coordinates from `data/events.json`.
[Adding a chapter](/website/chapter/) walks through the JSON shape.

The country-to-continent mapping in `data/continents.yaml` is what powers the continent grouping on both the chapters list and the directory filter.
A new country in a chapter file that is not in `continents.yaml` falls into "Other" — add it.

## Directory

Layouts: [`directory/list.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/directory/list.html), [`directory/single.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/directory/single.html).
Covered in detail at [The directory: where members live and how the site uses them](/website/admin_guide/directory-pipeline/).

Worth knowing here: the directory list page does no server-side filtering.
Every entry is on the page; Choices.js manages the dropdowns; Shuffle.js manages the visible cards; the filter logic intersects three data attributes serialised on each card.
This means the page is a single static HTML file, the filters work without JavaScript-aware infrastructure, and search engines see every member.

## Events

Layout: [`events/list.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/events/list.html).
Renders [FullCalendar](https://fullcalendar.io/) with the live events feed pulled from `rladies/meetup_archive`.
Two view toggles: list vs calendar, and week vs month.
A timezone selector defaults to the visitor's local timezone but lets them switch.

The events feed is fetched at build time via `resources.GetRemote`.
Hugo caches the response between builds for five minutes locally and longer in CI.
Because the calendar JS reads the events array from a Hugo template variable rather than fetching them at runtime, the page works without ever hitting the network from the visitor's browser — fast and resilient even when meetup.com has an outage.

Two output formats hang off `/events/`:

- `/events/index.xml` — RSS feed of upcoming events.  
- `/events/calendar.ics` — iCal feed for subscribing in calendar clients. The output format is configured in `config/_default/hugo.yaml` under `outputFormats.Calendar`.  

## Programs

Layout: [`programs/list.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/programs/list.html).
The programs index renders alternating-row sections, one per program.
Each program is a page bundle under `content/programs/<program-slug>/index.<lang>.md`.

Front matter for a program:

```yaml
---
title: "Mentoring"
description: |
  Markdown-supported description.
weight: 10
icon: "fa-solid fa-handshake"          # optional; if set, used instead of an image
image: "stock-image-id"                # if no icon, looks up data/stock_images.json
ctas:
  - label: "Apply to mentor"
    url: /form/mentor-apply/
    style: primary
  - label: "Read more"
    url: https://example.com/
    style: outline
---
```

The image lookup goes through `data/stock_images.json`, which carries credit and source information so attribution renders alongside the image.

## About-us subsections

`/about-us/our-story/` (layout: [`our-story/list.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/our-story/list.html)) is a long-scroll story page with alternating image rows and an Alpine-powered photo lightbox at the bottom.
Each story section is a page bundle under `content/about-us/our-story/`.

`/about-us/global-team/` (layout: [`global-team/list.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/global-team/list.html)) renders three groups: leadership cards (filtered by role), a role-grouped current team table plus a card grid, and an alumni grid with service dates.
All three read `data/global_team/` which is synced from Airtable weekly.
Cards link to the directory profile when `directory_id` is present.

`/about-us/involved/` (layout: [`involved/list.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/involved/list.html)) shows three "pillars" defined in the section's front matter, then alternating program rows for each subsection page, then a live "help wanted" GitHub issues feed at the bottom.
The issues feed is rendered client-side by [`assets/js/gh_helpwanted.js`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/assets/js/gh_helpwanted.js) — it queries the GitHub API for issues labelled `help wanted` across the org and renders them as cards.

`/about-us/faq/` (layout: [`faq/list.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/faq/list.html)) reads pages weighted under `content/about-us/faq/` and turns each one into a category section.
Each category's front matter holds an array of `items`, each with a `q` (question) and `a` (answer).
The page has category filter pills at the top (powered by Alpine `x-data`) and accordion-style expandable answers.
There is no markdown body — all content is structured front matter.

`/about-us/press/` (layout: [`press/list.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/press/list.html)) is a year-filterable list of press mentions.
Each entry is a page bundle with `title`, `date`, `source` (URL), and an optional PDF resource bundled into the folder.
Adding a press entry is documented at [Adding a mention of R-Ladies in the Press](/website/press/).

## Vacancies, packages, online-content

Three smaller layouts that ship alongside the main sections.

[`vacancies/single.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/vacancies/single.html) reads `data/global_team/.vacancies` (synced from Airtable) and renders open roles as cards with effort, skills, and apply buttons.
The page itself is content under `content/about-us/global-team/vacancies/index.<lang>.md`.

[`packages/single.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/packages/single.html) renders a hex-wall of R packages from the [awesome-rladies-creations](https://github.com/rladies/awesome-rladies-creations) feed.
Each hex links to the package and is annotated with author names that, when those authors have `directory_id`s, link to their profile pages.

[`online-content/single.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/online-content/single.html) renders the curated awesome-content feed (blogs, podcasts, newsletters, YouTube channels) as a card grid, with a per-type icon.

## Redirects

Layout: [`redirect/single.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/redirect/single.html).
Pages with `type: redirect` produce a short branded "Redirecting…" page that JavaScript-redirects the visitor to the target URL after a brief animation.
Aliased URLs work via Hugo's standard `aliases` front matter.
Documented in [Creating a pretty URL](/website/admin_guide/redirects/).

## A note on layout overrides

If you need a layout that does not exist, create it in the theme — that is the maintainable home.
If you need a one-off override that should not affect other sites consuming the theme, drop the file in the site repo's own `layouts/` tree at the matching path.
Hugo's lookup order will use the site copy first.

The site repo's `layouts/` directory currently has very few files.
That is a good thing.
Most of what is theme-y belongs in the theme.
