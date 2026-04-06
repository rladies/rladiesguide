---
title: "Cross-Repo Build Architecture"
linkTitle: "Build Architecture"
weight: 10
---

The R-Ladies website build is the most interconnected piece of our infrastructure.
Three repos feed into it, multiple secrets connect them, and a failure in one place can silently break the chain.
This page maps out how everything fits together.

## The build pipeline

When someone submits a new directory entry or blog post, the path to the live website looks like this:

1. **Airtable** receives a new submission (directory entry) or a contributor opens a PR (blog)  
2. **Source repos** (`directory` or `awesome-rladies-blogs`) process the data and create or update a PR  
3. The source repo **triggers a preview build** on `rladies.github.io` via `workflow_dispatch`  
4. **`rladies.github.io`** clones the source repos via SSH, builds the Hugo site, and deploys a preview to Netlify  
5. The preview build **posts a comment** back to the originating PR with the preview link  
6. Once the PR merges, the next **scheduled production build** (every 12 hours) picks up the changes  

## What connects to what

```text
directory                          rladies.github.io
  airtable-update.yml ──dispatch──▶ build-preview.yaml
  retrigger-website.yml ──dispatch──▶ build-preview.yaml
                                      │
awesome-rladies-blogs                 ├── clones directory (SSH)
  trigger-website.yaml ──dispatch──▶  ├── clones blogs (SSH)
                                      ├── deploys to Netlify
                                      └── comments on source PR
```

## Secrets involved

Each connection in the diagram above requires a specific secret:

| Connection | Secret | Stored in | Type |
|---|---|---|---|
| Source repo dispatches build | `GLOBAL_GHA_PAT` | Org secret | PAT |
| Website clones `directory` | `ssh_directoryy_repo` | `rladies.github.io` | SSH deploy key |
| Website clones `awesome-rladies-blogs` | `RLADIES_BLOGS_KEY` | `rladies.github.io` | SSH deploy key |
| Website comments on source PR | `GLOBAL_GHA_PAT` | Org secret | PAT |
| Website deploys preview to Netlify | `NETLIFY_AUTH_TOKEN` | `rladies.github.io` | API token |
| Website identifies Netlify site | `NETLIFY_SITE_ID` | `rladies.github.io` | Site ID |
| Global Team data pushed to main | `push-to-protected` | `rladies.github.io` | SSH deploy key |
| Global Team data pushed to main | `ADMIN_TOKEN` | Org secret | PAT |

## Production vs. preview builds

**Production** (`build-production.yaml`) runs on a 12-hour schedule.
It clones the latest `main` from `directory` and `awesome-rladies-blogs`, builds the full Hugo site, and deploys to GitHub Pages at `rladies.org`.

**Preview** (`build-preview.yaml`) is triggered on demand by the source repos.
It downloads the PR's artifact (updated entries), builds the site with those changes, and deploys to Netlify at a temporary preview URL.
It then posts a comment on the source PR with the preview link.

## When things break

The most common failure is an expired PAT — the `workflow_dispatch` call returns `HTTP 403` and the preview build never starts.
The source repo's workflow will fail at the "trigger build" step, but no one gets notified unless they check the Actions tab.

If the SSH deploy keys expire or are removed, the website build itself fails at the clone step.
This affects both preview _and_ production builds.

See the [GitHub PAT](../github-pat/), [Admin Token](../github-admin-token/), and [SSH Deploy Keys](../ssh-deploy-keys/) pages for rotation instructions.

## Netlify

We use two separate Netlify setups:

- **Website previews** — `rladies.github.io` deploys preview builds to Netlify using `NETLIFY_AUTH_TOKEN` and `NETLIFY_SITE_ID` (stored as repo-level secrets).
The preview URL is posted back to the originating PR.  
- **R-Ladies Guide** — `rladiesguide` is hosted on Netlify at `guide.rladies.org`.
The `.github` org repo has a `RLADIESGUIDE` secret containing a Netlify deploy hook URL that triggers a rebuild whenever the org-level repo is updated.  

The Netlify account credentials and site ownership should be documented with the team.
If you lose access to the Netlify account, preview deploys and the guide site will stop updating.
