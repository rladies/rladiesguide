---
title: Multi-lingual support
weight: 95
chapter: true
slides: true
menuTitle: Multi-lingual support
---

The RLadies+ website is built for a global audience.
Translation is a first-class feature, not an afterthought.
The site currently supports four languages — English, Spanish, Portuguese, and French — using [Hugo's multilingual mode](https://gohugo.io/content-management/multilingual/) and a small home-grown layer for placeholder generation.

Translations live in three different places, depending on what you are translating.

UI strings (menu items, button labels, "Read more", "Skip to main content") live in [`i18n/<lang>.yaml`](https://github.com/rladies/rladies.github.io/tree/main/i18n).
Templates pull these via `{{ i18n "key" }}`.

Page content (the prose of an article, the body of an FAQ) lives next to the English source as `index.<lang>.md` or `_index.<lang>.md`.

Localised data (month names, the "Read in" badge label) lives under `data/months/<lang>.yaml` and `data/read_in_lang.yaml`.

## What you see in production vs. development

Production currently exposes only the English site.
The Spanish, Portuguese, and French sites are built but not deployed — `disableLanguages: ["fr", "pt", "es"]` in [`config/production/hugo.yaml`](https://github.com/rladies/rladies.github.io/blob/main/config/production/hugo.yaml) keeps them out of the live deploy until enough translations are reviewed.

Locally, all four languages are active.
You will see the language picker in the footer and can switch between languages to preview translation work.

The reason for this gating is simple: shipping a half-translated site to a Spanish-speaking visitor is worse than shipping no Spanish site at all.
The "Read in" badge and the auto-translation banner are both honest signals about the state of a translation, but a fully untranslated production site would just be confusing.

## How auto-translation works

A page that exists in `index.en.md` but not in `index.es.md` is filled in automatically at build time by [`scripts/missing_translations.R`](https://github.com/rladies/rladies.github.io/blob/main/scripts/missing_translations.R).
The script copies the English source, sets `translated: auto` in the front matter, and saves it as `index.es.md`.
Hugo then renders the auto-translated copy under the Spanish URL with an orange banner telling the reader the page is auto-translated and inviting review.

The placeholder is real markdown content — the script does not actually translate it.
That happens through reviewers (volunteers, often, sometimes seeded by DeepL output on a feature branch).
Once a human has reviewed and translated the page, removing `translated: auto` from the front matter (or replacing it with `translated: true`) makes the banner disappear and marks the translation as ready.

This is how the site can claim multilingual support across hundreds of pages without having reviewed every translation.
Every URL exists in every language; the reader knows which copies are reviewed and which are awaiting translation.

## What goes where in the front matter

Three values for `translated` and what they mean:

`translated: true` — the page is reviewed and ready. No banner. This is the default.

`translated: auto` — the build script wrote this placeholder. Orange banner inviting review. The version under `hugo.IsProduction` does not show in language pickers (we do not want auto-translated copies to compete with reviewed ones in production).

`translated: false` — the page is intentionally untranslated. A "missing translation" banner shows, the language picker excludes this copy.

You usually do not write `translated: auto` by hand — the script does that.
You will encounter it on pages already populated by the build, and your job as a translation reviewer is to replace the auto content with reviewed content and remove the field.

## Working on translations

If you want to help with translation, two ways in:

[Reviewing translated content](/website/mulit-lingual/review/) walks through the workflow of finding a page, fixing the auto-translated text, and submitting a PR.
This is the most common path.

[Adding a new Site Language](/website/mulit-lingual/new/) covers the larger lift of adding support for a new language entirely — the i18n file, the languages config, the month names, the "Read in" string, the language code conventions.
This is rare and usually coordinated with leadership.

## A note on i18n key parity

Every key in `i18n/en.yaml` should exist in `i18n/<lang>.yaml`.
The [`i18n-check.yaml`](https://github.com/rladies/rladies.github.io/blob/main/.github/workflows/i18n-check.yaml) GitHub Action surfaces missing keys as a comment on every PR that touches `i18n/` or `content/`.
The check is informational rather than blocking — Hugo falls back to the English value when a translation is missing — but it is the prompt to translate the key the next time someone is working in that file.
