---
title: "The directory: where members live and how the site uses them"
weight: 3
menuTitle: Directory data flow
---

The RLadies+ directory looks, from the outside, like one page on the website: <https://rladies.org/directory/>.
A grid of member cards, three filters, a click-through to a profile.
Underneath, it is a small pipeline that touches three repositories, one Airtable base, two GitHub Actions, and a handful of cross-references that make the same person show up in five different places without anyone editing five different files.

If you are new to the site, the directory is the most important data source to understand.
It is also the one most likely to surprise you, because the canonical data does not live in this repo.

## Where the data actually lives

The website repository contains, deliberately, **only sample directory entries**: a handful of `data/directory/sample-*.json` files used so that `hugo server` produces a recognisable directory page locally.
Every real entry lives in a separate, private-ish repo: [rladies/directory](https://github.com/rladies/directory).
That repo holds one JSON file per member under `data/json/`, and an image for each member under `data/img/`.

Why a separate repo?

Privacy.
Members fill in a directory entry through an [Airtable form](https://rladies.org/form/directory-update).
Some of what they submit (their preferred contact method, sometimes their location) is information they want shown on the website but not surfaced in pull-request previews on a public website repo.
Keeping the directory in its own repo lets the team grant write access to the people who maintain it, run validations there, and pull the data into the website only at build time.

The website repo is public.
The directory repo holds personal information.
The split keeps those things in the right places.

## How the data gets from the directory repo into the build

There are two paths into a website build, and the GitHub Actions pick between them automatically.

The production build (on every push to `main` and on a 12-hour cron) checks out the latest `main` of `rladies/directory` using a deploy key, copies `data/json/` to `data/directory/` and `data/img/` to `assets/directory/`, and then runs Hugo:

```text
rm -r data/directory
checkout rladies/directory
cp -r tmpd/dir/data/json     data/directory
cp -r tmpd/dir/data/img/*    assets/directory
hugo -e production -d public
```

The preview build, used for pull requests and workflow dispatches, takes the same shape but with a twist.
If a maintainer of the directory repo wants to preview a single new entry without merging it, they can pass the run ID of a directory workflow run as the `directory` input.
The website action then downloads the entry artefact from that run instead of cloning the main branch:

```text
download-artifact name=entries from rladies/directory run_id=$directory
```

This is what makes it possible to test a new directory entry on a site preview before merging it anywhere.
Both paths converge on the same `data/directory/*.json` and `assets/directory/*` layout that Hugo expects.

The full flow looks like this:

```mermaid
flowchart LR
    A[Member fills<br>directory form] --> B[Airtable]
    B --> C[rladies/directory<br>JSON + image]
    C -->|push to main| D[Workflow run<br>creates artifact]
    C -->|merged| E[main branch]

    E -->|production action<br>checks out| F[website build<br>data/directory/]
    D -->|preview action<br>downloads artifact| F

    F --> G[/directory/ list]
    F --> H[/directory/&lt;id&gt;/ profile]
    F --> I[/about-us/global-team/]
    F --> J[Blog post bylines]
    F --> K[/news/ bylines]
    F --> L[Package hex wall]
```

That graph is the mental model.
Everything else on this page expands one of those arrows.

## What an entry looks like

A directory JSON has a stable shape.
The fields the templates actually read are these:

```jsonc
{
  "directory_id": "athanasia-mo-mowinckel",
  "name": "Athanasia Mo Mowinckel",
  "honorific": "Dr.",
  "image": "athanasia-mo-mowinckel.png",
  "location": { "country": "Norway", "state": "", "city": "Oslo" },
  "languages": ["English", "Norwegian"],
  "interests": ["R packages", "neuroimaging", "open science"],
  "categories": ["Developer", "Researcher"],
  "contact_method": ["email", "github"],
  "social_media": { "github": "drmowinckels", "bluesky": "drmowinckels.io" },
  "work": {
    "title": "Senior Engineer",
    "organisation": "Capgemini Engineering",
    "url": "https://capgemini.com"
  },
  "activities": {
    "r_groups": { "R-Ladies Oslo": "https://meetup.com/rladies-oslo" },
    "r_packages": { "ggseg": "https://github.com/ggseg/ggseg" }
  },
  "last_updated": "2025-04-12"
}
```

The `directory_id` field is the load-bearing one.
It is the slug Hugo uses to build the profile URL (`/directory/athanasia-mo-mowinckel/`), and it is the foreign key the rest of the site uses to refer to a member.

The image filename matches the JSON filename minus the extension, and lives under `assets/directory/`.
Hugo's image processing pipeline reads it from there, resizes it for cards (400×400 webp), profile portraits (1200×630 OG image), and hex-wall packages, and writes the resized files into `public/`.

## How the listing page works

[`themes/hugo-rladiesplus/layouts/directory/list.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/directory/list.html) reads `.Pages` (every directory entry as a Hugo page) and builds three things on the fly: the unique sorted list of interests, the unique sorted list of languages, and a country-grouped list of locations sorted by continent.
Those drive three [Choices.js](https://github.com/Choices-js/Choices) filter dropdowns at the top of the page.

The grid itself is a [Shuffle.js](https://github.com/Vestride/Shuffle) container.
Each card is a list item with `data-interests`, `data-languages`, and `data-location` attributes serialised as JSON.
When a filter changes, the page-level script reads the current Choices values and asks Shuffle to filter the visible cards by intersecting them with the data attributes.
There is no server-side search, no API call, no XHR — every entry is on the page already, and the filters just hide the ones that do not match.

The card itself is rendered by [`partials/funcs/directory/card.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/partials/funcs/directory/card.html).
Hovering it expands to show interests and a "View profile" link; clicking takes you to the single-page layout.

## How the profile page connects everything else

The profile page at `/directory/<directory_id>/` is rendered by [`directory/single.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/directory/single.html).
It shows the member's headshot, location, languages, interests, work affiliation, contact methods, and social links — straight from the JSON.

What is more interesting is what the page does *after* the static information.

It scans every regular page on the site looking for entries whose `author`, `editorial`, or `translator` front matter contains a `directory_id` matching this member.
Posts authored by them appear under "Posts".
Posts they edited appear under "Edited".
Posts they translated appear under "Translated", with the language of the translation noted.
This is how the directory becomes the central index for who-did-what across the blog and news.

It also pulls the [awesome-rladies-creations](https://github.com/rladies/awesome-rladies-creations) package list from a public raw URL and filters it for packages whose `authors[*].directory_id` matches.
If any of the member's packages are listed there, a hex-wall renders below the bio.

The link in the other direction — "Update your entry" — is a pretty URL.
It points to `/form/directory-update/?prefill_directory_id=<id>&prefill_request_type=update`, which redirects through the Airtable form with the existing entry pre-filled.
The same `directory_id` ties the website profile to the Airtable record that backs it.

## How directory IDs propagate to other content

Every place on the site that wants to credit a person uses the same pattern: include `directory_id` next to the name.

In a blog post:

```yaml
author:
  - name: Janani Ravi
    directory_id: "janani-ravi"
editorial:
  - name: Beatriz Milz
    directory_id: "beatriz-milz"
translator:
  - name: Riva Quiroga
    directory_id: "riva-quiroga"
```

In the global team data (synced from Airtable):

```yaml
mowinckel-athanasia:
  name: Athanasia Mowinckel
  role: [Website lead]
  directory_id: athanasia-mo-mowinckel
```

In the package list (synced from awesome-rladies-creations):

```json
{ "name": "ggseg", "authors": [{ "directory_id": "athanasia-mo-mowinckel" }] }
```

When `directory_id` is present, the byline becomes a link.
When it is missing, the byline renders as plain text.
[`partials/post/person-name.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/partials/post/person-name.html) is the one place this is decided, so the rule is uniform across blog posts, news posts, the global team page, and the package wall.

This is also why a member who has never written a post still has a profile that aggregates everything they have done.
The profile page is the destination; the `directory_id` is the link.

## Where the global team fits in

`/about-us/global-team/` reads `data/global_team/` rather than `data/directory/`.
The global team data has a different shape — `current` and `alumni` rather than one file per person — and is synced from a separate [Airtable base](https://airtable.com/) by the [`global-team.yml`](https://github.com/rladies/rladies.github.io/blob/main/.github/workflows/global-team.yml) Action on a weekly cron.
[`scripts/get_global_team.R`](https://github.com/rladies/rladies.github.io/blob/main/scripts/get_global_team.R) is the bridge.

But the global team entries also carry a `directory_id`.
The card components on the global-team page check for it and turn the card into a link to the directory profile when present.
A leader who has filled in their directory entry shows up linked from both the team list and the directory grid; a leader who has not gets a card with no link, and the omission is the prompt to fix it.

## Sample data and local previews

If you clone the website repo and run `hugo server`, you do not have access to the directory repo.
You also cannot run the production preview action.
Locally, you see whatever sample JSON files exist under `data/directory/sample-*.json` and whatever images sit beside them under `assets/directory/`.

That is enough to render the page, exercise the filters, see the layout, and confirm a styling change.
What it cannot do is reproduce production scale.
If you change the filter logic or the card markup, run a preview build through the GitHub Action so the change is exercised against the real, larger dataset before you merge.

The samples themselves are versioned in this repo.
If you find a bug that depends on a particular shape of entry, you can reproduce it by adding a new `sample-bug-repro.json` to the website repo, fixing the bug, and removing the sample once the fix lands.

## When something goes wrong

The most common directory-related failure is a JSON schema problem.
The [`check-jsons.yaml`](https://github.com/rladies/rladies.github.io/blob/main/.github/workflows/check-jsons.yaml) action runs [`scripts/validate_jsons.R`](https://github.com/rladies/rladies.github.io/blob/main/scripts/validate_jsons.R) on every PR; the schemas live under [`scripts/json_shema/`](https://github.com/rladies/rladies.github.io/tree/main/scripts/json_shema).
A failing build there will leave a comment on the PR pointing at the offending file and field.
Almost always: a missing required field, an unknown social network, a date in the wrong format.

The next most common failure is an image that is not where the templates expect it.
A directory entry whose `image` field references `jane-doe.png` needs `assets/directory/jane-doe.png` to exist.
If it does not, Hugo silently renders the entry without an image — the build does not fail, but the card looks wrong.
The directory repo's own validations catch most of these, but not all.

The third pattern is a stale `directory_id`.
Someone changes their slug in the directory repo (because the file was renamed), but a blog post in the website repo still references the old one.
The byline goes back to plain text, and nobody notices for weeks.
There is no automated check for this yet — if you are renaming an entry in the directory repo, grep the website repo for references to the old slug before you merge.
