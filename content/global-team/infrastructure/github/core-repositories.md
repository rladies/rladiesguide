---
title: "Core repositories"
linkTitle: "Core repos"
weight: 20
---

The `rladies` org hosts dozens of repositories — chapter presentation archives, one-off workshop materials, retired experiments.
A much smaller set runs the project day to day.
If any one of these stops working, something visible to the community breaks.
This page is the short list, with one paragraph per repo and a pointer to wherever it is documented in detail.

The full repo list is at `github.com/orgs/rladies/repositories`; the eight below are the ones to know about first.

## The eight that matter

### `rladies.github.io`

The website at `rladies.org` — Hugo source, build workflows, and the deployment pipeline that pulls together the chapter directory, blog feed, and Global Team data.
This is the most interconnected repo in the org: it clones two other private repos via SSH on every build, dispatches workflows in those repos, and posts comments back when builds finish.
For the full topology, see [Build Architecture]({{% relref "../build-architecture" %}}).

### `directory`

The chapter directory data source.
Airtable submissions flow in, a workflow turns them into a PR, and merging the PR triggers a preview build on `rladies.github.io`.
The interaction with the website is documented in [Build Architecture]({{% relref "../build-architecture" %}}); the PAT it uses to dispatch is in [GitHub PAT]({{% relref "../github-pat" %}}).

### `awesome-rladies-blogs`

The blog post feed.
Contributors open PRs adding their blog URL; on merge, the website rebuilds and the post appears on the community blog page.
Same cross-repo dispatch pattern as `directory` — see [Build Architecture]({{% relref "../build-architecture" %}}).

### `global-team`

Onboarding and offboarding automation for new chapter organisers and Global Team members.
The workflows here are the entry point for "add this person to the right teams" and "remove this person cleanly" — they use the [Admin Token]({{% relref "../github-admin-token" %}}) for the org-admin operations and are triggered by the human flow described in [Onboarding new chapter organizers]({{% relref "/global-team/onboarding" %}}).

### `meetup_archive`

A scheduled archive of chapter and event data pulled from the Meetup.com API every 12 hours.
The full `events.json` produced here feeds both chapter activity reporting and the [Jinx]({{% relref "/global-team/jinx" %}}) content indexer.
Secret management lives in [Meetup API]({{% relref "../meetup-api" %}}).

### `jinx`

The R package, Cloudflare Worker, content indexer, and workflow definitions that together make up the [Jinx]({{% relref "/global-team/jinx" %}}) bot.
This is the home of the GitHub App and the long-term destination for any workflow that currently uses a personal access token.
The architecture, permissions, and adoption guide live in the Jinx section — see [For developers]({{% relref "/global-team/jinx/for-developers" %}}).

### `branding-materials`

The visual identity files — logos, hex sheets, the Affinity and Canva source files, the `brand.yml` for Quarto and Shiny, and the [Branding-guidelines.pdf](https://github.com/rladies/branding-materials/blob/main/Branding-guidelines.pdf).
Most chapter and Global Team branding questions resolve to a file in this repo.
See the [Branding]({{% relref "/branding" %}}) section for which file to grab when.

### `rladiesguide`

This guide.
A Hugo site hosted on Netlify at `guide.rladies.org`, deployed via a webhook stored as `RLADIESGUIDE` on the org `.github` repo (see [Build Architecture]({{% relref "../build-architecture" %}})).
PR-time checks run a Hugo build (`check-build.yaml`), a broken-link sweep (`link-check.yaml`), i18n coverage (`i18n-check.yaml`), and Zenodo metadata validation.
Quarterly releases, contributor greetings, and acknowledgements are handled by [Jinx]({{% relref "/global-team/jinx" %}}).

## Repos that are core-adjacent

A few more repos appear regularly in the workflow logs without quite belonging on the "if it breaks, the community notices" list:

- `awesome-rladies-creations` — content and package issue automation, Slack RSS feed for community-made R artefacts.
- `meetupr` — the R client library for the Meetup API, used by `meetup_archive` and documented in [Meetup API]({{% relref "../meetup-api" %}}).
- `.github` — the org-level config repo; holds the `RLADIESGUIDE` Netlify deploy hook secret and any org-wide community health files.

If you maintain one of these and find yourself adding a new workflow or a new secret, document it in the appropriate runbook so the next person doesn't have to reverse-engineer it from the workflow YAML.

## What's deliberately not in this section

Repos belonging to individual chapters (`meetup-presentations_*`, location-specific workshop repos) are not part of the org's operational infrastructure.
They're the org's _content_, not its plumbing.
The plumbing is the eight repos above.
