---
title: "Apps and bots"
linkTitle: "Apps & bots"
weight: 40
---

The list of GitHub Apps installed on the `rladies` org is short on purpose.
Every installed app gets some level of access to repository contents, issues, or org metadata, and each one is something that has to be reviewed when org owners change.
The fewer apps in the list, the easier the review.

To see what is currently installed, open `github.com/organizations/rladies/settings/installations` (or **Settings → Third-party Access → GitHub Apps** from the org page).
Only org owners can view this page.

## Jinx

The headline integration is [Jinx]({{% relref "/global-team/jinx" %}}) — the GitHub App that backs the org's automation.
Workflows that need to comment on PRs, dispatch builds across repos, invite new members, or push commits as a bot identity mint short-lived installation tokens through Jinx instead of using a personal access token.

Jinx is installed on the repos listed in ["Where Jinx runs today"]({{% relref "/global-team/jinx#where-jinx-runs-today" %}}).
The App's permission set, the secrets it needs, and the pattern for adopting it in a new workflow are in [For developers]({{% relref "/global-team/jinx/for-developers" %}}).

Adding Jinx to a new repo is the recommended path whenever a workflow would otherwise reach for `GLOBAL_GHA_PAT` or `ADMIN_TOKEN`.
The org-level Jinx secrets (`JINX_APP_ID`, `JINX_PRIVATE_KEY`) are already available to every repo in the org — see [Jinx for developers]({{% relref "/global-team/jinx/for-developers" %}}) for the full secret inventory and the App permissions Jinx already holds.

Adopting Jinx in a new workflow looks like this:

```yaml
jobs:
  do-the-thing:
    runs-on: ubuntu-latest
    steps:
      - name: Get a Jinx installation token
        id: app-token
        uses: actions/create-github-app-token@v3
        with:
          client-id: ${{ secrets.JINX_APP_ID }}
          private-key: ${{ secrets.JINX_PRIVATE_KEY }}
          owner: rladies

      - name: Use it
        env:
          GH_TOKEN: ${{ steps.app-token.outputs.token }}
        run: gh pr comment $PR_NUMBER --body "hello from Jinx"
```

Two details that catch people out: the input is `client-id`, not `app-id`, and the token is only valid for one hour after the step runs.
If the new workflow needs a permission Jinx doesn't yet hold on the target repo (Discussions, Pages, anything outside the table in [Jinx for developers]({{% relref "/global-team/jinx/for-developers#what-jinx-is-allowed-to-touch" %}})), ask an org admin to expand Jinx rather than spin up a parallel token.

If Jinx is accidentally uninstalled from a repo, every workflow on that repo that mints an installation token will start failing with `Resource not accessible by integration`.
The fix is to re-install: open `github.com/organizations/rladies/settings/installations`, click **Configure** next to Jinx, and add the repo back to the "Repository access" list.
The org-level secrets stay put — only the installation grant needs restoring.

## Dependabot

Dependabot is GitHub-native and likely active across the core repos for security alerts and version updates, although configuration is per-repo rather than org-wide.
Where it is enabled, it opens PRs against `main` like any other contributor and goes through the same branch-protection review (see [Branch protection]({{% relref "branch-protection" %}})).

To check or change the org-wide defaults, go to `github.com/organizations/rladies/settings/security_analysis` (**Settings → Code security** from the org page).
Per-repo configuration lives in `.github/dependabot.yml` on each repo.

When a Dependabot PR can't merge — usually because branch protection requires an approving review and no human got to it — the fix is one of: a human reviewer approves and merges it manually, or `dependabot.yml` is updated to assign the PR to a real reviewer so the notification surfaces.
Disabling branch protection to let Dependabot through is not a fix.

TODO: confirm Dependabot is enabled on each of the core repos and whether any repo has a checked-in `dependabot.yml` worth promoting to a template.

## Other integrations

The deliberately short list of additional integrations:

- **GitHub Actions** — not really an "app" in the same sense, but worth naming: every workflow in the org runs through Actions, which means org-level Actions settings (allowed actions, runner permissions, secret access scope) are part of this surface area.
  Review at `github.com/organizations/rladies/settings/actions` (**Settings → Actions → General** from the org page).
  TODO: confirm the current "Allowed actions" policy and whether it restricts third-party actions or permits any reusable workflow.
- **Netlify** — connected to `rladies.github.io` for preview builds and to `rladiesguide` for the guide site.
  The OAuth grant lives at the user level, not as an org-installed app, but it shows up in the deploy chain.
  See [Build Architecture]({{% relref "../build-architecture" %}}) for how preview deploys reach Netlify.

## What's not installed

A short list of things that are _not_ on the org and which should stay off without a clear reason:

- No third-party code review or security-scanning apps with write access.
- No CI services beyond GitHub Actions.
- No bots from individual contributor accounts with org-wide install scope.

Keeping this list small is the whole point.
Anything added here should be reviewable at a glance by the next person who inherits org-owner duties.
