---
title: "Adding a new Site Language"
menuTitle: "Site Language"
weight: 7
---

Adding a new language to the site is rare but mechanical.
You need to register the language with Hugo, copy a few translation files, and start the long work of reviewing the auto-translated content.
This page is the checklist.

Before you start, settle on the [ISO 639-1 two-letter code](https://www.w3schools.com/tags/ref_language_codes.asp) for the language — `it` for Italian, `de` for German, `ja` for Japanese.
That code is what you will use everywhere.

## Files to create or edit

### 1. Register the language with Hugo

Edit [`config/_default/languages.yaml`](https://github.com/rladies/rladies.github.io/blob/main/config/_default/languages.yaml) and add the new language:

```yaml
it:
  params:
    languageName: "Italiano"
    weight: 5
    description: "RLadies+ è un'organizzazione mondiale per promuovere la diversità di genere nella comunità R"
```

`weight` controls the order in the language picker.
`description` is the meta description used on the home page in this language.

If the language needs a translated menu (slugs in the local language), add a `menus` entry alongside `params`.
French is the example — it lives in the same file under `fr:` and replaces the English URL slugs (`/events/` → `/evenements/`, `/chapters/` → `/chapitres/`, etc.).
Most languages can leave the URL slugs in English and just translate the labels via i18n.

### 2. Copy and translate the i18n strings

```bash
cp i18n/en.yaml i18n/it.yaml
```

Open `i18n/it.yaml` and translate the right-hand side of every key.
Keep the keys in English.
Keep YAML indentation intact.

This is the file that drives "Read more", "Skip to main content", "Find a Chapter", every menu label, every form label.
A complete file is a few hundred lines.
You can submit a partial file and let the i18n-check action surface what is missing — Hugo falls back to the English value for any key not yet translated.

### 3. Add the "Read in" badge label

Edit [`data/read_in_lang.yaml`](https://github.com/rladies/rladies.github.io/blob/main/data/read_in_lang.yaml) and add a line:

```yaml
it: "Leggi in italiano"
```

This is the label that shows on the translation badge of a blog post when an Italian translation exists.

### 4. Add the localised month names

```bash
cp data/months/en.yaml data/months/it.yaml
```

Translate each month name to the new language.
The site uses these whenever a date is rendered into prose ("12 Giugno 2026" rather than "12 June 2026").

### 5. (Optional) Decide on production exposure

A new language is not exposed in production until you remove its code from `disableLanguages` in [`config/production/hugo.yaml`](https://github.com/rladies/rladies.github.io/blob/main/config/production/hugo.yaml).
Leave it in `disableLanguages` while the translations are mostly auto-generated and unreviewed.
Remove it once enough pages have been reviewed that the language site is genuinely useful to a native speaker.

This is a judgement call.
There is no fixed threshold.
Speak with the translation team about it.

## Working on the translation

Once the language is registered, the [`scripts/missing_translations.R`](https://github.com/rladies/rladies.github.io/blob/main/scripts/missing_translations.R) script will produce auto-translated placeholders for every English page on the next build.
The placeholders are real markdown copies of the English source, with `translated: auto` in their front matter and the orange auto-translation banner shown to readers.

From there, the work is page-by-page review: replace the English content with the translated version, remove `translated: auto`, push.
The full workflow is in [Reviewing translated content](/website/mulit-lingual/review/).

## On the file naming convention

Every translated content file takes the form `index.<lang>.md` or `_index.<lang>.md`.
The English file is `index.en.md`, the Italian copy is `index.it.md`.
The two-letter code in the filename must match the code you registered in `languages.yaml`.

If you rename a content directory, the rename happens for every language at once — the bundle is one folder, with multiple `index.<lang>.md` files inside.

## Submitting the change

Open a PR with all the files above.
Tag [@rladies/translation](https://github.com/orgs/rladies/teams/translation) for review.
The i18n-check action will run and report on coverage; the build action will run and produce a preview that includes the new language site.
Click through the preview to confirm the language picker, the translated menu items, the auto-translated content, and the date formatting all look right.

A team member will merge once the review is complete.
The new language will appear in the language picker on local previews immediately and in production once `disableLanguages` is updated.
