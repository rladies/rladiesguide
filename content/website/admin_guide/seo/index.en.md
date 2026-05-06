---
title: SEO, social previews, and accessibility
weight: 7
menuTitle: SEO & accessibility
---

You will probably never write SEO metadata by hand on this site.
The theme generates Open Graph cards, Twitter cards, hreflang alternates, JSON-LD structured data, and the site's robots/sitemap configuration from front matter and site params.
This page explains where the generation happens so you can debug a wrong-looking Slack preview or fix a missing alt-text complaint without grepping templates.

## What gets generated, and from what

[`partials/head/meta.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/partials/head/meta.html) is the shared head block for every page.
It computes:

- the page `<title>` — combines the page title and the site title, deduplicating when they match.  
- the meta description — uses the page's `description`, falling back to the site description.  
- the Open Graph image, with width and height — resolves in this order: page `image.path` (resized to 1200×630), first image resource on the page, site `params.ogImage`. The result is the URL Slack, LinkedIn, and Bluesky show in a preview card.  
- `og:type` — `article` for blog and news pages, `website` everywhere else.  
- `article:published_time`, `article:modified_time`, `article:author`, `article:tag` — only on `blog` and `news` pages.  
- Twitter card metadata.  
- Hreflang alternates — every translation of the current page, plus `x-default`, when the site is multilingual.  

Everything else is downstream of three things: the page's front matter, the site params in `config/_default/params.yaml`, and the languages config.

## How to fix a bad preview

A Slack or LinkedIn preview that shows the wrong image, the wrong title, or no description usually means one of these:

The page's front matter has no `image.path`, no resources of type image, and the site default OG image is missing.
Open the page bundle, add an image, and reference it as `image.path: "filename.png"`.

The page has an `image.path` but the file is not actually present in the bundle.
Hugo's `Resources.Get` returns nothing, the OG resolution falls through to the site default, the preview shows that.
Check the bundle directory.

The page's front matter description is empty.
The site description is fine for a homepage but not for a blog post — add a `description:` to the post.

The platform has cached the old preview.
Use [LinkedIn's Post Inspector](https://www.linkedin.com/post-inspector/), [Twitter's Card Validator](https://cards-dev.twitter.com/validator), or paste the URL into a private chat to bust the cache.

## Structured data

[`partials/head/jsonld.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/partials/head/jsonld.html) emits a JSON-LD `<script>` for each page describing the organisation (using the ROR ID set in `params.yaml`) and the article (when it is one).
This is what lets Google show authorship and publication date in the search result and gives the site machine-readable identity for archives like Wikidata.

You usually do not need to do anything with this.
If a page is showing up in search without an authored byline and you think it should, check that the post has a `directory_id`-bearing author entry — that is what threads through into the JSON-LD `author.url`.

## hreflang and the language picker

Every page is rendered with `<link rel="alternate" hreflang="<lang>">` tags pointing at every translation, plus `<link rel="alternate" hreflang="x-default">` pointing at the default-language version.
Search engines use this to serve a Spanish reader the Spanish copy and an English reader the English copy.

The language picker in the footer is built from `site.Sites` (the list of language sites) cross-referenced with `Page.AllTranslations` (the list of translated copies of this specific page).
A language without a translated copy falls back to the home page in that language.
The picker only appears when the site is built with multiple languages active.

In production, `disableLanguages` in `config/production/hugo.yaml` currently excludes `es`, `pt`, and `fr` until those translations are reviewed.
This means the picker also disappears in production.
That is intentional — see [Multi-lingual support](/website/mulit-lingual/) for the rationale.

## Accessibility

Three pieces are worth flagging because they are easy to break.

### Alt text on images

The render hook for images requires alt text — but only because the markdown convention `![alt](src)` does.
A missing alt is `![](image.png)`, which the [`blog-lint.yaml`](https://github.com/rladies/rladies.github.io/blob/main/.github/workflows/blog-lint.yaml) action catches and fails the build on.
The Lighthouse audit in [`lighthouse.yaml`](https://github.com/rladies/rladies.github.io/blob/main/.github/workflows/lighthouse.yaml) also fails the PR if any image on key pages is missing alt text.

For images that are purely decorative (a brand mark beside a heading, an inline icon), use `alt=""` deliberately.
Empty alt is signal to screen readers that the image is decoration, which is correct.

### Skip links and landmarks

The base layout starts with a `Skip to main content` link that becomes visible on focus, and the `<main>` element has `id="main"` so the link works.
The navigation is in `<nav>`, the footer in `<footer>`, and headings follow a single-`<h1>` per-page rule.

If you are adding a new layout, do not forget the `<main id="main">`.
Without it the skip link silently does nothing.

### Reduced motion

The CSS respects `prefers-reduced-motion`.
Animations on the home page and on the redirect page check the media query and disable the spinning, fading, scaling motion when the user has asked for less of it.
If you add a custom animation, wrap it in `@media (prefers-reduced-motion: no-preference) { ... }` or guard with `prefers-reduced-motion: reduce`.

## Robots and sitemaps

The site has `enableRobotsTXT: true` in `hugo.yaml`, which means Hugo generates `/robots.txt` from [`themes/hugo-rladiesplus/layouts/robots.txt`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/robots.txt).
Hugo also generates `/sitemap.xml` automatically.
Both are served at the site root in production.

If you want to disallow a path from search engines (e.g., a draft section or a redirect-only path), edit the `robots.txt` template in the theme.

## Plausible analytics

The site uses [Plausible](https://plausible.io/) for cookieless analytics, loaded asynchronously from `<head>`.
Plausible records page views, referrers, and the country a request came from at a coarse-grained level.
It does not set cookies, does not use fingerprinting, and does not require a consent banner.

If a stakeholder asks "do we have stats", point them at the Plausible dashboard rather than rolling another analytics tool into the site.
If a stakeholder asks for a tool that requires consent, the answer is no — that conflicts with the no-cookie design that lets us skip the banner.
