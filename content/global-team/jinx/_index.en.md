---
title: "Jinx"
linkTitle: "Jinx"
weight: 5
chapter: false
aliases:
  - /coordination/jinx/
---

Someone opens a pull request in the directory repo.
Within seconds, a website preview build kicks off in `rladies.github.io`, a comment appears on the PR with a link to follow the build, and the contributor sees a friendly note welcoming them.
None of that needed a person.
It needed Jinx.

Jinx is the RLadies+ organization's GitHub App.
Think of it as a small bot account that lives in the org and lends its identity to our automations.
When a workflow needs to comment on a pull request, kick off a build in another repository, or invite a new contributor to a team, it asks Jinx for a short-lived token and acts on Jinx's behalf.
The comment shows up as coming from `Jinx[bot]`.
The bot is the familiar -- the little creature that does the errands so the witches don't have to.

## Why we use a bot instead of personal tokens

For a long time RLadies+ workflows ran on personal access tokens.
That worked, but it had two annoying properties.
First, those tokens belong to one person.
When the person leaves, takes a long break, or simply forgets to rotate, the tokens expire and a workflow somewhere goes quiet -- usually at the worst possible moment.
Second, personal tokens tend to drift toward broad scopes: it is easier to grant "all my repos" than to scope down per-workflow, and what starts as a quick fix becomes a key with far more reach than it needs.

A GitHub App fixes both.
Jinx has no human owner.
The credentials live as org-level secrets, not in any individual's account.
Each workflow run mints a token that expires in an hour and is scoped to the specific repos that workflow needs to touch.
Nothing long-lived sits around waiting to leak.

## Two ways to talk to Jinx

### Slash commands on GitHub

In any repo where Jinx is installed, you can leave a comment on an issue or pull request that starts with `/jinx`.
The bot reads the comment, runs the matching command, and replies inline.

### Slash commands on Slack

Jinx also lives in the RLadies+ organiser Slack workspace.
Type `/jinx` followed by a command in any channel, DM, or conversation with the Jinx app.
A Cloudflare Worker receives the request, sends Jinx a fun acknowledgement, and dispatches the command to a GitHub Actions workflow.
When the command finishes, Jinx posts the result back to Slack.

The same commands work in both places.
The difference is where the response goes -- GitHub issue comment or Slack message.

## Commands

Here is what Jinx knows how to do today.

### Information (replies with content)

| Command | What it does |
|---|---|
| `/jinx help` | Show all available commands |
| `/jinx report weekly` | Org-wide activity summary (commits, PRs, issues) |
| `/jinx report monthly` | Same, for the past 30 days |
| `/jinx report chapters` | Chapter health: active vs inactive, months since last event |
| `/jinx gha-dashboard` | GitHub Actions CI status across all repos |
| `/jinx analytics` | Org-wide contributor and commit trends |
| `/jinx website-analytics [period]` | Plausible website stats (7d/30d/month/6mo/12mo) |
| `/jinx contributors [repo]` | Contributor list for a repo |
| `/jinx contributors org` | Top contributors org-wide |
| `/jinx events <chapter>` | Upcoming events for a chapter |
| `/jinx cfp list` | Open calls for proposals |
| `/jinx translate status` | Translation coverage across languages |
| `/jinx translate validate [lang]` | Check translation placeholders |

These commands return the information directly.
No GitHub issues are created.

### Actions (does something, replies with a link)

| Command | What it does |
|---|---|
| `/jinx invite @user to <team>` | Invite a user to the org and a team, creates onboarding issue |
| `/jinx offboard @user from <team>` | Start offboarding, creates tracking issue |
| `/jinx chapter-setup <city> <country>` | Create a chapter setup checklist issue |
| `/jinx chapter-update <city> <country>` | Create a chapter update issue |
| `/jinx blog-add <url>` | Auto-create a blog entry from a URL |
| `/jinx cfp add <conf> <deadline> <url>` | Track a new call for proposals |
| `/jinx cfp recommend <conf> @speaker` | Recommend a speaker for a conference |
| `/jinx contributors update [repo]` | Update the contributors list via PR |
| `/jinx events sync` | Sync and publish the chapter event summary |
| `/jinx announce <post-url>` | Announce a blog post on social media |
| `/jinx remind stale` | Nudge stale onboarding/offboarding issues, report links |

These commands perform an action (create an issue, send an invite, post an announcement) and reply with a link to what was created.

The canonical command list is always at [`/jinx help`](https://github.com/rladies/jinx/blob/main/inst/commands/help.md).

## How Slack commands work

The Slack integration has three pieces.

**1. The Slack App** -- a Slack application called Jinx installed in the organiser workspace.
It provides the `/jinx` slash command and the bot identity for posting messages.

**2. The Cloudflare Worker** -- a small JavaScript function at `rladies-jinx.workers.dev` that receives slash commands from Slack.
It verifies the request, immediately responds with a friendly quip so the user is not left waiting, and dispatches the command to GitHub.
The `/jinx help` command is handled entirely in the worker for instant response.
The worker mints its own Jinx GitHub App installation token -- no personal access tokens involved.

**3. The GitHub Actions workflow** -- a single `commands.yml` workflow in the `rladies/jinx` repo that handles both GitHub issue comments and Slack dispatches.
It runs the command through the jinx R package, captures the result as a string, and routes it to the right destination -- a GitHub issue comment or a Slack message.

```
User types: /jinx report weekly
           |
           v
  Cloudflare Worker
  - verifies Slack signature
  - replies "🐾 Padding over to handle /jinx report weekly..."
  - fires repository_dispatch to rladies/jinx
           |
           v
  GitHub Actions (commands.yml)
  - installs R + jinx package
  - runs: jinx::execute_command(cmd)
  - captures result string
  - posts to Slack via post_slack_message()
           |
           v
  User sees the weekly report in Slack
```

If anything fails along the way, Jinx always responds -- either with a helpful error message or a link to the workflow logs.
Sensitive data (Slack channel IDs, response URLs, usernames) is masked in the CI logs.

## Where Jinx runs today

Jinx is installed across the RLadies+ GitHub org.
Every automated comment, PR, issue, and commit across these repos shows up as `Jinx[bot]`.

- [**rladies/jinx**](https://github.com/rladies/jinx) -- the app's home, command workflows, scheduled jobs, Cloudflare Worker code
- [**rladies/global-team**](https://github.com/rladies/global-team) -- onboarding, offboarding, stale-issue reminders, status reports
- [**rladies/rladies.github.io**](https://github.com/rladies/rladies.github.io) -- preview builds, blog lint, JSON validation, Lighthouse audits, contributor welcomes, merge automation
- [**rladies/directory**](https://github.com/rladies/directory) -- Airtable sync, preview-build dispatch, purge workflows, outreach
- [**rladies/awesome-rladies-creations**](https://github.com/rladies/awesome-rladies-creations) -- content/package issue automation, URL checks, Slack RSS feed
- [**rladies/meetupr**](https://github.com/rladies/meetupr) -- README rendering
- [**rladies/rladiesguide**](https://github.com/rladies/rladiesguide) -- contributor greetings, acknowledgements, quarterly releases

If you maintain a repo not on this list and find yourself reaching for a personal access token, that is a good signal to use Jinx instead.

## Under the hood

A workflow that wants to act as Jinx follows a three-step pattern.

First, it mints an installation token:

```yaml
- name: Generate app token
  id: app-token
  uses: actions/create-github-app-token@v1
  with:
    app-id: ${{ secrets.JINX_APP_ID }}
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

| Scope | Permission | What it enables |
|---|---|---|
| Repository | Issues -- read & write | PR/issue comments, slash command replies |
| Repository | Pull Requests -- read & write | PR comments, reviews, status |
| Repository | Contents -- read & write | Reading files, creating branches, pushing commits |
| Repository | Actions -- read & write | Cross-repo workflow dispatch |
| Organization | Members -- read & write | `/jinx invite` and `/jinx offboard` |
| Organization | Administration -- read | Reading team membership |

If a new workflow needs a permission outside this list, ask the org admins to expand Jinx rather than introduce a parallel token.
A single bot identity is easier to audit and rotate than several.

## Secrets and tokens

All Jinx secrets are stored at the **org level** and available to all repos:

| Secret | What it is |
|---|---|
| `JINX_APP_ID` | The GitHub App's numeric ID |
| `JINX_PRIVATE_KEY` | The app's PEM private key |
| `SLACK_ORGANISER_TOKEN` | Slack bot token (`xoxb-...`) for the organiser workspace |
| `SLACK_COMMUNITY_TOKEN` | Slack bot token for the community workspace |
| `CLOUDFLARE_API_TOKEN` | For auto-deploying the Cloudflare Worker |

The Cloudflare Worker has its own secrets (set via `npx wrangler secret put`):

| Secret | What it is |
|---|---|
| `SLACK_SIGNING_SECRET` | Verifies requests actually come from Slack |
| `JINX_APP_ID` | Same app ID, used to mint installation tokens |
| `JINX_PRIVATE_KEY` | Same private key, in PKCS8 format |

## CI and quality checks

The jinx repo runs several checks on pull requests:

- **R-CMD-check** -- standard R package check, only triggers on R package file changes
- **Cloudflare Worker validation** -- `wrangler deploy --dry-run` on worker file changes, catches syntax/bundling errors
- **goodpractice** -- runs `goodpractice::gp()` and posts suggestions as a Jinx PR comment
- **pkgdown** -- builds the documentation site on push to main

## Adding Jinx to a new workflow

1. Confirm Jinx is installed on the repo: check [the org's app settings](https://github.com/organizations/rladies/settings/installations).
2. Confirm `JINX_APP_ID` and `JINX_PRIVATE_KEY` are available as org-level secrets.
3. Add the `actions/create-github-app-token` step to your workflow.
4. Use `steps.app-token.outputs.token` wherever you would have used a token.
5. For comments and commits, make sure they go through the app token so they show as `Jinx[bot]`.

If Jinx is not yet installed on the repo, ping someone with org-admin access.

## When something breaks

A few failure modes that have actually happened.

**"A JSON web token could not be decoded"** -- the private key in `JINX_PRIVATE_KEY` is not a valid PEM.
This usually means the contents were truncated when pasted, or a repo-level secret is shadowing the org-level one.
Check `gh secret list --repo rladies/<repo>` for repo-level overrides and delete them if they exist.

**"Resource not accessible by integration"** -- the token was minted, but the action is outside Jinx's permission set on the target repo.
Either Jinx is not installed on the target, or the permission needed is not yet granted.
Check the app installation list and the permission table above.

**"channel_not_found" in Slack** -- the bot needs to be in the channel to post.
For DMs, open a conversation with the Jinx app first.
For private channels, `/invite @Jinx`.
Public channels work if the bot has `chat:write.public` scope.

**Slack says "app did not respond"** -- the Cloudflare Worker is down or the signing secret is wrong.
Check the worker logs with `npx wrangler tail` from the `jinx` repo.

**No response from Jinx in Slack after the quip** -- the GitHub workflow failed.
Check the [Jinx Actions runs](https://github.com/rladies/jinx/actions) for `repository_dispatch` events.
If the workflow fails, Jinx tries to post the error back to Slack with a link to the logs.

## The jinx R package

The command logic lives in the [`jinx` R package](https://rladies.github.io/jinx/).
Key design principle: `execute_command()` is a pure function that returns a string.
It has no knowledge of GitHub or Slack.
The caller (the workflow YAML) handles routing the response to the right place.

This means you can run any Jinx command from an R console:

```r
cmd <- jinx::parse_command("/jinx report weekly")
result <- jinx::execute_command(cmd)
cat(result)
```

Full package documentation is at [rladies.github.io/jinx](https://rladies.github.io/jinx/).

## A note on the name

The bot account is `jinx-familiar` -- Jinx the app, familiar the role.
Familiars carry messages, fetch ingredients, and generally handle the small magic so the witches can focus on the big magic.
That is roughly what we want the bot to do for us, too.
