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

Jinx is the R-Ladies organization's GitHub App.
Think of it as a small bot account that lives in the org and lends its identity to our automations.
When a workflow needs to comment on a pull request, kick off a build in another repository, or invite a new contributor to a team, it asks Jinx for a short-lived token and acts on Jinx's behalf.
The comment shows up as coming from `jinx-familiar[bot]`.
The bot is the familiar — the little creature that does the errands so the witches don't have to.

## Why we use a bot instead of personal tokens

For a long time R-Ladies workflows ran on personal access tokens.
That worked, but it had two annoying properties.
First, those tokens belong to one person.
When the person leaves, takes a long break, or simply forgets to rotate, the tokens expire and a workflow somewhere goes quiet — usually at the worst possible moment.
Second, personal tokens tend to drift toward broad scopes: it is easier to grant "all my repos" than to scope down per-workflow, and what starts as a quick fix becomes a key with far more reach than it needs.

A GitHub App fixes both.
Jinx has no human owner.
The credentials live as org-level secrets, not in any individual's account.
Each workflow run mints a token that expires in an hour and is scoped to the specific repos that workflow needs to touch.
Nothing long-lived sits around waiting to leak.

## What you'll see Jinx doing

Across the R-Ladies repos, Jinx shows up in a few shapes.

In pull request comments, Jinx posts build notifications, contributor welcomes, and review checklists.
In issues, Jinx responds to slash commands typed by maintainers — the kind you might recognize from other bots like `/lgtm` or `/approve`.
In the background, Jinx runs scheduled jobs: weekly chapter reports, Airtable syncs, stale-entry reminders, contributor lists.
And in a handful of workflows, Jinx steps across repositories — for example, when the directory repo needs to trigger a preview build in `rladies.github.io`, that cross-repo handshake goes through Jinx.

## Talking to Jinx with slash commands

In any repo where Jinx is installed, you can leave a comment on an issue or pull request that starts with `/jinx`.
The bot reads the comment, runs the matching workflow, and replies inline.

A few of the commands that exist today:

`/jinx invite @username to <team>` — onboards a new global team volunteer to a specific working group.  
`/jinx offboard @username from <team>` — the reverse, when someone is rotating off.  
`/jinx announce <post-url>` — drops a blog post into the announcements queue.  
`/jinx events <chapter-slug>` — pulls upcoming events for a chapter.  
`/jinx events sync` — refreshes the Meetup/Luma event mirror.  
`/jinx report weekly` — generates the weekly activity digest.  
`/jinx help` — lists what Jinx currently knows how to do.  

The canonical list lives in the [jinx package commands source](https://github.com/rladies/jinx/blob/main/R/commands.R).
If a command you expect is not in the help output, the workflow it points to may need to be installed in the repo you are working in.

## Under the hood

A workflow that wants to act as Jinx follows the same three-step pattern every time.

First, it mints an installation token using the [actions/create-github-app-token](https://github.com/actions/create-github-app-token) action, passing in the Jinx App ID and private key.
Second, it scopes that token to just the repositories the workflow needs to touch — `directory,rladies.github.io`, for example, and not the whole org.
Third, it uses the token wherever it would otherwise have used `GITHUB_TOKEN` or a personal access token.

The whole thing looks like this:

```yaml
- name: Mint Jinx app token
  id: app_token
  uses: actions/create-github-app-token@v1
  with:
    app-id: ${{ secrets.JINX_APP_ID }}
    private-key: ${{ secrets.JINX_PRIVATE_KEY }}
    owner: rladies
    repositories: directory,rladies.github.io

- name: Comment on the PR
  env:
    GH_TOKEN: ${{ steps.app_token.outputs.token }}
  run: gh pr comment ${{ github.event.pull_request.number }} --body "Hi from Jinx"
```

The token returned by the first step is good for one hour.
Once the workflow finishes, the token is revoked and can never be reused.

## What Jinx is allowed to touch

Jinx asks for the minimum set of permissions it needs across the org, no more.
The current grants are:

| Scope | Permission | What it enables |
|---|---|---|
| Repository | Issues — read & write | PR/issue comments, slash command replies |
| Repository | Pull Requests — read & write | PR comments, reviews, status |
| Repository | Contents — read | Reading files for reports and checks |
| Repository | Actions — read & write | Cross-repo `gh workflow run` dispatch |
| Organization | Members — read & write | `/jinx invite` and `/jinx offboard` |
| Organization | Administration — read | Reading team membership |

If a new workflow needs a permission outside this list, the cleanest path is to ask the org admins to expand Jinx rather than introduce a parallel token.
A single bot identity is easier to audit and rotate than several.

## Adding Jinx to a new workflow

Adopting Jinx in a repo it is already installed on is short:

1. Confirm Jinx is installed on the repo by checking [the org's app settings](https://github.com/organizations/rladies/settings/installations) — the jinx entry should list your repository.  
2. Confirm `JINX_APP_ID` and `JINX_PRIVATE_KEY` are available as org-level secrets that your repo can read.  
3. Add the `actions/create-github-app-token` step to your workflow, scope it with the `repositories:` field, and use `steps.app_token.outputs.token` wherever you would have used a token.  

If Jinx is _not_ yet installed on the repo you need, ping someone with org-admin access — the install is a click in the GitHub UI.

## When something breaks

A few failure modes that have actually happened, and what they mean.

> Failed to create token: Invalid keyData

The private key in `JINX_PRIVATE_KEY` is not a valid PEM.
Almost always this means the contents were truncated when pasted into the secret, or the `-----BEGIN RSA PRIVATE KEY-----` / `-----END RSA PRIVATE KEY-----` headers were accidentally left out, or a client secret was pasted instead of a private key.
Regenerate the key from the app settings, copy the entire `.pem` file contents including headers and newlines, and re-paste.
GitHub Apps support multiple active private keys, so you can roll a new one without taking the old one down until you have confirmed the new one works.

> Resource not accessible by integration

The token was minted, but the action it tried to perform is outside Jinx's permission set on the target repo.
Either Jinx is not installed on the target, or the permission needed is not yet granted to the app.
Check the app installation list and the permission table above.

> Bad credentials

The App ID is wrong, or the private key does not match the App ID.
This shows up most often when there are two Apps in the org and the wrong pair of secrets is being used together.

## A note on the name

The bot account is `jinx-familiar` — Jinx the app, familiar the role.
Familiars carry messages, fetch ingredients, and generally handle the small magic so the witches can focus on the big magic.
That is roughly what we want the bot to do for us, too.
