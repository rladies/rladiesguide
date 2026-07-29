---
title: Theme assets and npm bundling
weight: 2
menuTitle: Theme assets & npm
---

When you clone the website repo and run `hugo server`, the site just works.
You did not install Node.
You did not run `npm install`.
The chapter map renders.
The events calendar renders.
The dark-mode toggle works.
That is on purpose, and the way it is achieved is worth understanding before you start changing things.

## The constraint that shaped the build

Most of the people who contribute to the RLadies+ website are not full-time front-end engineers.
They are R-using volunteers who want to fix a typo, add a chapter entry, or write a blog post.
Asking them to install a JavaScript toolchain just to preview a markdown change would push most of them away.

So the theme imposes a contract: anyone who only writes content needs only Hugo.
Anyone who changes the theme's CSS, updates a vendor library, or adds a new front-end dependency runs npm — but only inside the theme directory, and they commit the resulting bundles so the next person does not have to.

This is enforced by where things live and what is in `.gitignore`.

## Where the build artefacts go

Inside the theme:

```
themes/hugo-rladiesplus/
├── package.json              # npm scripts and dependencies
├── node_modules/             # installed by `npm install`, gitignored
├── assets/
│   ├── css/
│   │   ├── main.css          # Tailwind v4 entry, hand-written
│   │   ├── components/       # hand-written component CSS, imported by main.css
│   │   └── vendor/           # GENERATED — committed
│   │       ├── tailwind.css  # compiled by `tailwindcss --minify`
│   │       └── choices.min.css
│   ├── js/
│   │   ├── _d3map.entry.js          # esbuild entry for the chapter map
│   │   ├── _fullcalendar.entry.js   # esbuild entry for the events calendar
│   │   ├── chapter-filter.js        # hand-written, used by /chapters/
│   │   ├── counter.js               # hand-written, used everywhere
│   │   ├── darkmode.js              # hand-written, inlined in <head>
│   │   ├── map-init.js              # hand-written, reads the d3map bundle
│   │   ├── ...
│   │   └── vendor/                  # GENERATED — committed
│   │       ├── alpine.min.js
│   │       ├── choices.min.js
│   │       ├── d3map.bundle.min.js
│   │       ├── fullcalendar.bundle.min.js
│   │       ├── mermaid.min.js
│   │       └── shuffle.min.js
│   └── scss/
│       └── vendor/fontawesome/      # GENERATED — committed
└── static/
    └── webfonts/                    # GENERATED — committed
        ├── fontawesome/             # Font Awesome woff2
        └── google-fonts/            # Poppins, Inconsolata woff2
```

Everything labelled GENERATED is built by an npm script and committed to the repo.

## The build script, line by line

The full pipeline lives in [`themes/hugo-rladiesplus/package.json`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/package.json).
The `build` script chains together every step:

```text
npm run clean              wipe the generated folders
npm run setup              recreate empty target folders
npm run build:css          tailwindcss --minify -i main.css -o vendor/tailwind.css
npm run build:fullcalendar esbuild bundles FullCalendar plugins
npm run build:d3map        esbuild bundles d3-geo, world-atlas, iso-3166-1
npm run sync:fontawesome   copies woff2 + scss out of node_modules into the theme
npm run sync:choices       copies choices.js
npm run sync:shuffle       copies shufflejs
npm run sync:alpine        copies Alpine
npm run sync:mermaid       copies the mermaid bundle
npm run sync:fonts         copies Poppins + Inconsolata woff2 files
```

The `postinstall` script runs `npm run build` automatically.
Running `npm install` once gets you a complete, ready-to-commit set of vendor assets.

The two `esbuild` steps deserve a closer look.
The chapter map uses [d3-geo](https://github.com/d3/d3-geo) to project a world atlas, [topojson-client](https://github.com/topojson/topojson-client) to parse the topology, and [iso-3166-1](https://github.com/lukehorvat/iso-3166-1) to translate country codes.
That is several npm packages that the browser cannot load directly.
[`assets/js/_d3map.entry.js`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/assets/js/_d3map.entry.js) imports them, attaches the parts the templates need to `window.__d3map`, and esbuild rolls the lot into a single `vendor/d3map.bundle.min.js`.
The events calendar follows the same pattern via [`_fullcalendar.entry.js`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/assets/js/_fullcalendar.entry.js).

## How Hugo Pipes uses the artefacts

Once the bundles exist, Hugo Pipes is what serves them.
The relevant pieces are in [`partials/head/head.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/partials/head/head.html) and [`partials/footer/scripts.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/partials/footer/scripts.html):

```go-html-template
{{ $tw := resources.Get "css/vendor/tailwind.css" }}
{{ $css := slice $tw
  | resources.Concat "rladiesplus-bundle.min.css"
  | minify
  | fingerprint
}}
<link rel="stylesheet" href="{{ $css.RelPermalink }}" integrity="{{ $css.Data.Integrity }}">

{{ $fa := resources.Get "scss/fontawesome.scss" | toCSS | minify | fingerprint }}
<link rel="stylesheet" href="{{ $fa.RelPermalink }}" media="print" onload="this.media='screen'">
```

The Tailwind CSS is concatenated, minified, and fingerprinted.
The FontAwesome SCSS is compiled at build time by the Hugo extended binary's libsass support, then minified and fingerprinted.
The integrity hash goes into the `<link>` tag for SRI.

Section-specific JavaScript follows the same pattern.
The events calendar bundle is only loaded by `events/list.html`, the chapter map only by templates that include `partials/map.html`, the directory filter only by `directory/list.html`.
Pages that do not need a feature do not download its bundle.

## When you actually need to run npm

Three situations:

You changed something in [`assets/css/main.css`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/assets/css/main.css) or one of the files under `assets/css/components/`.
You added a new vendor library to `dependencies` in `package.json`.
You bumped a version in `package.json`.

In all three cases, the workflow is the same.
From the theme directory:

```bash
cd themes/hugo-rladiesplus
npm install      # the postinstall hook rebuilds everything
git status       # confirm the regenerated files
git add assets/ static/webfonts/
git commit -m "rebuild theme vendor bundles"
```

If you only edited `main.css` and want a faster loop while iterating, run just the CSS step:

```bash
npm run build:css
```

The Tailwind CLI watches your `main.css` for `@import` directives, scans every Hugo template under `themes/hugo-rladiesplus/layouts/` for class names (Tailwind v4 does this with zero config), and emits a CSS file containing only the utilities your templates actually use.
This is why the production CSS is small: there is no "all of Tailwind" in the bundle, just the rules the site actually references.

## When you do not need to run npm

You added or edited a markdown post.
You added a new chapter JSON.
You translated an i18n key.
You added a new layout template that does not introduce new utility classes Hugo cannot find.

You can safely commit and push.
The site will rebuild against the existing vendor bundles.

## What is gitignored, and why it matters

Inside the theme: `node_modules/` and `package-lock.json`-related lockfile cruft you create incidentally.
The vendor bundles themselves — the things npm produces — are committed.
That is the inversion that makes this work.

If you accidentally check in `node_modules/` (a few hundred megabytes), the build will still pass but the diff review will be unpleasant.
If you accidentally git-ignore the vendor bundles, every clone will build a broken site until someone notices.

## What about icons and fonts

[Font Awesome](https://fontawesome.com/) is the icon set.
The theme syncs the official Free SCSS into `assets/scss/vendor/fontawesome/` and the woff2 files into `static/webfonts/fontawesome/`.
[`assets/scss/fontawesome.scss`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/assets/scss/fontawesome.scss) imports the parts the site uses.
Hugo compiles that SCSS at build time.

[Poppins](https://fonts.google.com/specimen/Poppins) is the body font, [Inconsolata](https://fonts.google.com/specimen/Inconsolata) is the code font.
Both are self-hosted via [Fontsource](https://fontsource.org/), copied into `static/webfonts/google-fonts/`, and preloaded in `<head>` to keep the site off Google's CDN and avoid the layout shift you get from waiting for fonts.

If you want to swap fonts: change the `@fontsource/...` dependency in `package.json`, run `npm install`, update `--font-sans` / `--font-mono` in `assets/css/main.css`, and update the `<link rel="preload">` paths in `partials/head/head.html`.

## Plausible analytics

The site uses [Plausible](https://plausible.io/) for privacy-respecting analytics.
The script is loaded asynchronously from `https://plausible.io/js/script.js` with `defer`, gated to the `rladies.org` domain.
There is no cookie banner because there is no cookie.
If you ever need to disable analytics — for a fork, for a preview, for legal reasons — comment out the two lines at the bottom of `partials/head/head.html`.
