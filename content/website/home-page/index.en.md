---
title: "Editing the home page"
menuTitle: "Home page"
weight: 7
---

The [rladies.org](https://rladies.org) home page is composed of a stack of sections — hero, impact, chapters, get-involved, featured members, latest posts, sponsors. Each section's text and imagery live in **frontmatter**, not in i18n files or template code, so contributors can change copy and visuals without touching the theme.

## Where the home content lives

```
content/
├── _index.en.md       ← English home page
├── _index.es.md       ← Spanish
├── _index.fr.md       ← French
├── _index.pt.md       ← Portuguese
└── _img/              ← all home-page images
    ├── og-image.png
    ├── women-meeting-1.jpg
    └── …
```

Each language file is independent — title, descriptions, CTAs, and image alt text are translated by editing the file for that language. Image *files* are shared across languages (page resources are bundle-wide), so only `_img/` needs one copy.

## The frontmatter shape

A trimmed example, showing one of every kind of block:

```yaml
---
image:
  path: _img/og-image.png
  alt: "Description of the OG preview image."

hero:
  headline: 'Promoting Gender Diversity in the R Community'
  subheadline: 'A worldwide organization …'
  cta_chapters: 'Find a Chapter'
  cta_involved: 'Get Involved'

impact:
  title: 'Our Global Impact'
  subtitle: 'Building an inclusive R community worldwide'
  stats:
    - icon: fa-globe
      label: 'Countries'
      count_source: chapter_countries
    - icon: fa-map-location-dot
      label: 'Chapters'
      count_source: active_chapters
      color: blue
    - icon: fa-calendar-days
      label: 'Events'
      count_source: events_past
      color: rose
  photos:
    - path: _img/gender-spectrum-office-group.jpeg
      alt: 'Group of people of varying genders working in an office'
      credit: '[The Gender Spectrum Collection](https://genderspectrum.vice.com/)'
    - path: _img/women-office-meeting.jpg
      alt: 'Women having an office meeting around a table'

chapters:
  title: 'Chapters'
  cta: 'Browse All Chapters'

involved:
  title: 'Get Involved'
  subtitle: 'There are many ways to be part of the RLadies+ community'
  cards:
    - icon: fa-users
      color: blue
      title: 'Join a Chapter'
      desc: 'Find your local RLadies+ chapter…'
      cta:
        label: 'Find Chapters'
        url: chapters/
      image:
        path: _img/women-meeting-1.jpg
        alt: 'Group of women of color having a lively meeting around a table'
    - icon: fa-microphone
      title: 'Give a Talk'
      desc: 'Share your knowledge…'
      cta:
        label: 'Get Involved'
        url: about-us/involved/
      image:
        path: _img/microphone-speaker.jpg
        alt: 'Condenser microphone in the foreground with a woman reading in the background'
    - icon: fa-hand-holding-heart
      color: rose
      title: 'Support Us'
      desc: 'Help sustain our mission…'
      cta:
        label: 'Donate'
        url: 'https://www.paypal.com/donate/?hosted_button_id=…'
        external: true
        button_class: btn-accent-rose
      image:
        path: _img/gender-spectrum-party.jpeg
        alt: 'Group of friends of varying genders celebrating at a dinner party'
        credit: '[The Gender Spectrum Collection](https://genderspectrum.vice.com/)'

featured:
  title: 'Featured Members'
  subtitle: 'Meet some of the people in our community'
  cta: 'Browse Directory'

blog:
  title: 'Latest Posts'
  subtitle: 'News, tutorials, and stories from our community'
  cta_posts: 'All Posts'
  cta_news: 'All News'
  cta_press: 'All Press'

sponsors:
  title: 'Sponsors'
  subtitle: 'Organizations that support our mission'
  cta: 'Become a Sponsor'
---
```

Most fields are plain strings — change them and re-deploy. The two collections worth knowing in detail are `impact.stats` and `involved.cards`.

## Stats (`impact.stats`)

Each stat is one card on the "Our Global Impact" row:

```yaml
stats:
  - icon: fa-globe              # Font Awesome class
    label: 'Countries'           # text under the number
    count_source: chapter_countries  # which counter to show
    color: blue                  # optional: blue | rose | (default purple)
```

- **`icon`** — any [Font Awesome 6 solid icon](https://fontawesome.com/icons) class (`fa-…`).
- **`label`** — the visible label (translated per language).
- **`count_source`** — which dynamic count to display. Currently supported values:
  - `chapter_countries` — number of countries with at least one chapter
  - `active_chapters` — number of currently active chapters
  - `events_past` — total events ever held (pulled live from the meetup archive)
  
  Adding a new `count_source` value requires a small template change — ask the website team.
- **`color`** — `blue`, `rose`, or omit for the default purple.

Stats render in the order listed. To add or remove a stat, edit the YAML — no template change needed.

## Cards (`involved.cards`)

Each card is one tile in the "Get Involved" row:

```yaml
cards:
  - icon: fa-users               # Font Awesome class for the round icon
    color: blue                  # optional: blue | rose | (default purple)
    title: 'Join a Chapter'
    desc: 'Find your local RLadies+ chapter…'
    cta:
      label: 'Find Chapters'
      url: chapters/             # internal path or full URL
      external: true             # optional, set when url is off-site
      button_class: btn-accent-rose  # optional CSS class for the button
    image:
      path: _img/women-meeting-1.jpg
      alt: 'Group of women of color having a lively meeting around a table'
      credit: 'optional source/photographer'
```

- **`icon`** and **`color`** — same options as stats.
- **`cta.url`** — internal paths (e.g. `chapters/`) are passed through `relLangURL` so they pick up the language prefix automatically. For external links, set `external: true`; the rendered link gets `target="_blank" rel="noopener"`.
- **`cta.button_class`** — overrides the default `btn-outline`. We use `btn-accent-rose` for the "Donate" card so it stands out in pink.
- **`image`** — same shape as everywhere else on the site (see [Images on the website](../images/)).

Cards render in the order listed. To rearrange, swap them in the YAML; to remove a card, delete its entry; to add a fourth, append a new one (the layout is a 3-column grid by default — adding a fourth card may need a CSS tweak).

## What's *not* in frontmatter

A handful of generic UI strings stay in the i18n files (`i18n/en.yaml`, `i18n/es.yaml`, …) because they're reused across the site, not just on the home page:

- `countries`, `chapters`, `events` — used as fallback labels when a `stats` entry doesn't define one
- `photo_credit` — the literal word "Photo" before a credit line

If you need to change one of those, edit the i18n file and translate to all four languages.

## Adding a new language

1. Copy `content/_index.en.md` to `content/_index.<lang>.md`.
2. Translate every string value (titles, subtitles, descriptions, CTA labels, image `alt` texts, `credit` text if attribution differs).
3. Leave the `path:` values, `count_source` values, `icon` classes, and `color` values **unchanged** — those are not user-facing text.
4. Add the language to `config/_default/languages.yaml`.

See [Multi-lingual content]({{% relref "/website/mulit-lingual" %}}) for the broader translation workflow.
