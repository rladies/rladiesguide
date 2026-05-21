---
title: "For developers"
menuTitle: "For developers"
weight: 60
---

<img src="/img/jinx/coding.svg" alt="Jinx the witch's cat, at a laptop" width="140" align="right" style="margin: 0 0 1rem 1.5rem;">

This page is for the org members who write workflows or hack on Jinx itself.
If you just want to know what `/jinx` can do, [Commands]({{< relref "commands" >}}) is the place.
For a quick map of which code runs in which runtime, see ["Where the code runs"]({{< relref "/global-team/jinx#where-the-code-runs" >}}) on the landing page.

## Under the hood

A workflow that wants to act as Jinx follows a three-step pattern.

First, it mints an installation token:

```yaml
- name: Generate app token
  id: app-token
  uses: actions/create-github-app-token@v3
  with:
    client-id: ${{ secrets.JINX_APP_ID }}
    private-key: ${{ secrets.JINX_PRIVATE_KEY }}
    owner: rladies
```

Second, it uses the token wherever it would otherwise have used `GITHUB_TOKEN` or a personal access token:

```yaml
- name: Comment on the PR
  env:
    GH_TOKEN: ${{ steps.app-token.outputs.token }}
  run: gh pr comment ${{ github.event.pull_request.number }} --body "Hi from Jinx"
```

Third, for actions that create visible bot activity (comments, commits, PRs), use the token so it shows as `Jinx[bot]`:

```yaml
- name: Post checklist
  uses: actions/github-script@v9
  with:
    github-token: ${{ steps.app-token.outputs.token }}
    script: |
      await github.rest.issues.createComment({ ... })
```

For git commits, use the Jinx identity:

```yaml
git config user.name "Jinx[bot]"
git config user.email "jinx@rladies.org"
```

The token returned by the first step is good for one hour.
Once the workflow finishes, the token is revoked.

## What Jinx is allowed to touch

Jinx asks for the minimum set of permissions it needs across the org.

| Scope        | Permission                    | What it enables                                   |
| ------------ | ----------------------------- | ------------------------------------------------- |
| Repository   | Issues -- read & write        | PR/issue comments, slash command replies          |
| Repository   | Pull Requests -- read & write | PR comments, reviews, status                      |
| Repository   | Contents -- read & write      | Reading files, creating branches, pushing commits |
| Repository   | Actions -- read & write       | Cross-repo workflow dispatch                      |
| Organization | Members -- read & write       | `/jinx invite` and `/jinx offboard`               |
| Organization | Administration -- read        | Reading team membership                           |

If a new workflow needs a permission outside this list, ask the org admins to expand Jinx rather than introduce a parallel token.
A single bot identity is easier to audit and rotate than several.

## Secrets and tokens

### Org-level GitHub secrets

All Jinx secrets are stored at the **org level** and available to every repo:

| Secret                  | What it is                                                          |
| ----------------------- | ------------------------------------------------------------------- |
| `JINX_APP_ID`           | The GitHub App's client ID (passed to `create-github-app-token` v3) |
| `JINX_PRIVATE_KEY`      | The app's PEM private key                                           |
| `SLACK_ORGANISER_TOKEN` | Slack bot token (`xoxb-...`) for the organisers workspace           |
| `SLACK_COMMUNITY_TOKEN` | Slack bot token for the community workspace                         |
| `AIRTABLE_API_KEY`      | PAT for Airtable reads (chapter directory, invites, events)         |
| `SLACK_INVITE_LINK`     | Shared invite URL used when emailing new chapter members            |
| `CLOUDFLARE_API_TOKEN`  | For auto-deploying the Cloudflare Worker and running the indexer    |

### Cloudflare Worker secrets

The worker has its own secrets, set via `npx wrangler secret put`:

| Secret                                                    | What it is                                                                |
| --------------------------------------------------------- | ------------------------------------------------------------------------- |
| `SLACK_SIGNING_SECRET`                                    | Verifies requests actually come from Slack                                |
| `SLACK_ORGANIZER_TEAM_ID` / `SLACK_COMMUNITY_TEAM_ID`     | Allowlist of Slack workspaces Jinx will respond in                        |
| `SLACK_ORGANIZER_BOT_TOKEN` / `SLACK_COMMUNITY_BOT_TOKEN` | Per-workspace bot tokens for the Slack Web API                            |
| `JINX_APP_ID` / `JINX_PRIVATE_KEY`                        | Same app credentials, used to mint installation tokens for slash dispatch |
| `AIRTABLE_WEBHOOK_SECRET`                                 | Shared secret on the Airtable invite webhook                              |
| `AIRTABLE_PAT`                                            | PAT used to read base metadata and update invite records                  |

### Indexer secret

The `bot-index-content.yml` workflow needs one extra secret beyond the GitHub App and Cloudflare token:

| Secret            | What it is                                                                    |
| ----------------- | ----------------------------------------------------------------------------- |
| `YOUTUBE_API_KEY` | YouTube Data API v3 key, used to list videos from the RLadies+ Global channel |

## CI and quality checks

The jinx repo runs several checks on pull requests:

- **R-CMD-check** -- standard R package check, only triggers on R package file changes
- **Cloudflare Worker validation** -- `wrangler deploy --dry-run` on worker file changes, catches syntax/bundling errors
- **Worker unit tests** -- `vitest` over the JS modules in `worker/src/`
- **goodpractice** -- runs `goodpractice::gp()` and posts suggestions as a Jinx PR comment
- **i18n validation** -- checks translation placeholders for the in-package strings
- **pkgdown** -- builds the documentation site on push to main

## Adding Jinx to a new workflow

1. Confirm Jinx is installed on the repo: check [the org's app settings](https://github.com/organizations/rladies/settings/installations).
2. Confirm `JINX_APP_ID` and `JINX_PRIVATE_KEY` are available as org-level secrets (they are, for every repo in the org).
3. Add the `actions/create-github-app-token@v3` step to your workflow (note: it expects `client-id`, not `app-id`).
4. Use `steps.app-token.outputs.token` wherever you would have used a token.
5. For comments and commits, make sure they go through the app token so they show as `Jinx[bot]`.

If Jinx is not yet installed on the repo, ping someone with org-admin access.

## The jinx R package

The command logic lives in the [`jinx` R package](https://rladies.github.io/jinx/).
Key design principle: `cmd_execute()` is a pure function that returns a string.
It has no knowledge of GitHub or Slack.
The caller (the workflow YAML) handles routing the response to the right place.

This means you can run any Jinx command from an R console:

```r
cmd <- jinx::cmd_parse("/jinx report weekly")
result <- jinx::cmd_execute(cmd)
cat(result)
```

Functions follow a `<module>_<verb>[_<object>]` schema -- `cmd_*`, `gh_*`, `slack_*`, `i18n_*`, `cfp_*`, `gha_*`, `gt_*` -- so autocomplete groups them cleanly.
Full package documentation is at [rladies.github.io/jinx](https://rladies.github.io/jinx/).
