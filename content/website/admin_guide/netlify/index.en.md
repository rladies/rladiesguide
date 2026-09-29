---
title: "Netlify"
linkTitle: "Netlify"
weight: 12
aliases:
  - /global-team/infrastructure/netlify/
---

Every PR on the directory and blog repos gets a preview build whose URL comes from a single Netlify account, and the guide you are reading right now is served from a sibling site in that same account.
That is the entire footprint: two sites, one login, a handful of secrets in the right repos.

## What lives on Netlify, and what doesn't

The production website at `rladies.org` is _not_ on Netlify — it deploys to GitHub Pages from the `gh-pages` branch of `rladies/rladies.github.io`.
Netlify only handles previews of the website and the production deploy of this guide.
The [Build Architecture]({{% relref "/global-team/infrastructure/build-architecture" %}}) page has the full cross-repo picture; this page covers the account side.

The split is deliberate.
Previews need a fresh URL per build and a generous build minute budget; production needs to be boring, predictable, and on infrastructure with a different blast radius than the preview pipeline.
Keeping them on different providers means a Netlify outage does not take `rladies.org` down, and a GitHub Pages incident does not block contributors from previewing their PRs.

## Sites in the account

There are two sites under the RLadies+ Netlify account.

| Site                                             | What it serves                 | Triggered from                                    | Secret(s)                                          |
| ------------------------------------------------ | ------------------------------ | ------------------------------------------------- | -------------------------------------------------- |
| `rladies-preview` (TODO confirm exact site name) | Per-PR previews of the website | `rladies.github.io` `build-preview.yaml`          | `NETLIFY_AUTH_TOKEN`, `NETLIFY_SITE_ID` (repo)     |
| `guide.rladies.org`                              | This guide (production)        | `rladiesguide` builds, kicked off by a build hook | `RLADIESGUIDE` (deploy hook URL on `.github` repo) |

The two sites are configured very differently.
The preview site is driven from CI: the website workflow ships a built `public/` directory to Netlify with the CLI and asks for an alias under the site.
The guide is driven from Netlify itself: Netlify clones the repo, runs `hugo` (the version and command live in `netlify.toml` at the repo root), and publishes the result whenever the build hook fires.
Both patterns are documented further down.

## Account ownership

The Netlify account login is the role mailbox — the same shared role-account mailbox used for Cloudflare and Google Workspace.
The password and any 2FA backup codes live in 1Password (see [1Password]({{% relref "/global-team/infrastructure/1password" %}})) in the Shared vault, under an entry named `Netlify`.

The reasoning matches the rest of the shared-account pattern: when a leadership member rolls off, the Netlify account does not roll off with them.
The downside is that a shared password is a shared password, so we rotate it whenever someone with access to the Shared vault leaves.

## Team membership

Netlify's Free tier limits collaborators on the account.
TODO: confirm the current plan tier (Free or Starter) and the exact collaborator cap.
In practice, day-to-day operations only need the role mailbox login — individual Global Team members do not need their own Netlify seats to ship preview builds or update the guide, because all of that runs through CI and the deploy hook.

To add a collaborator: `app.netlify.com` → top-left team switcher → **Team settings → Members → Invite member**, enter the email address, pick a role (**Owner**, **Developer**, **Reviewer**, or **Billing admin**), and send the invite.
**Owner** can change billing and remove other members; **Developer** can ship deploys and edit site settings; **Reviewer** can browse but not change anything; **Billing admin** only sees the billing surface.
Before inviting, check the current member count against the tier cap — Free tier seats fill quickly, and an unused seat blocks a real one.

TODO: list current collaborators on the account, if any beyond the role mailbox, and why each one is there.
Anyone added should have a clear operational reason; "convenience" is not enough on a tier with a hard collaborator cap.

## Tokens and deploy hooks

Three secrets connect Netlify to the rest of the build pipeline.

`NETLIFY_AUTH_TOKEN` is a personal access token issued from the Netlify dashboard, stored as a repository secret on `rladies/rladies.github.io`.
It is what the preview build uses to authenticate the Netlify CLI when it uploads the built site.

`NETLIFY_SITE_ID` is the identifier of the preview site, also stored on `rladies/rladies.github.io`.
It is not really a secret — the site ID does not grant access on its own — but treating it as a secret keeps the rotation story consistent with the auth token: both live in the same place, both get reviewed at the same time.

`RLADIESGUIDE` is a Netlify _build hook_ URL stored as a secret on the `rladies/.github` repo.
A POST to that URL with no body tells Netlify to rebuild the guide.
The `.github` repo's automation calls it when content changes here have been merged.

Cross-link the [GitHub PAT page]({{% relref "/global-team/infrastructure/github-pat" %}}) for the general pattern we use for secret storage and rotation in `rladies` repos.

Build hooks are deliberately preferred over long-lived API tokens for this kind of inter-service trigger.
A build hook is scoped to one site and to one action — triggering a build — and rotating it is a single click in the Netlify dashboard.
A leaked API token, by contrast, can do anything the user can do across every site on the account.

## Custom domains and TLS

`guide.rladies.org` is the only custom domain in the account.
The DNS record that points at Netlify lives in Cloudflare (TODO cross-link Cloudflare runbook once merged); Netlify itself provisions and renews the TLS certificate via Let's Encrypt.
Renewals are automatic and silent under normal circumstances.

When a renewal fails, Netlify makes it visible in two places.
The dashboard shows a red banner on `app.netlify.com` → site → **Domain management → HTTPS** reading either `SSL certificate has expired` or `DNS verification failed`, and the same message lands by email at the role mailbox.
The recovery path follows the banner: click **Verify DNS configuration** first; if that comes back green, click **Renew certificate** and the new cert provisions in a minute or two.
If **Verify DNS configuration** comes back red, the problem is upstream — the Cloudflare CNAME for the subdomain has drifted off Netlify's expected target, and the fix is on Cloudflare (TODO cross-link Cloudflare runbook), not here.
Come back and click **Renew certificate** once the DNS check is green.

## Build minutes and usage

The Free tier caps build minutes per calendar month across the whole account.
Both sites draw from the same bucket, so a runaway preview site eats into the guide's headroom and vice versa.

Two places to check usage.
The fast read is `app.netlify.com` → **Sites overview** → the **Usage** card in the top right, which shows current-month minutes against the cap at a glance.
The detailed view is **Team settings → Billing → Usage**, which breaks usage down by site and shows the historical trend.
Netlify emails the role mailbox at 90% utilisation; if that mail arrives, treat it as an incident, not a notification — there is roughly one week of normal traffic left before builds start failing.

## Deploy notifications

Both sites benefit from a Slack or email ping on deploy success and failure, so a broken build does not sit unnoticed.
Set them up at site → **Site configuration → Build & deploy → Deploy notifications → Add notification**.
Pick the event (deploy started, deploy succeeded, deploy failed, deploy locked), pick the transport (email, Slack incoming webhook, outgoing webhook), and paste the destination.
TODO: confirm which Slack channels currently receive deploy notifications and whether both sites are wired up.
A reasonable default is `#website` for the preview site and `#global-team-tech` (or similar) for the guide, with deploy-failed events going to both.

## Operational tasks

### Rotating `NETLIFY_AUTH_TOKEN`

1. Log in at `app.netlify.com` using the credentials from [1Password]({{% relref "/global-team/infrastructure/1password" %}})
2. Top-right avatar → **User settings → Applications → Personal access tokens → New access token**
3. Name it `rladies-github-io-ci`, set an expiration (one year is fine), click **Generate token**, copy the value immediately
4. Update the GitHub secret: `gh secret set NETLIFY_AUTH_TOKEN --repo rladies/rladies.github.io` and paste the new token when prompted
5. Verify by opening any PR on `rladies/directory` and manually triggering the `airtable-update` workflow from the Actions tab, then watch the `build-preview` workflow run on `rladies/rladies.github.io` and confirm its Netlify deploy step completes with a preview URL posted back to the PR
6. Once green, revoke the old token in the same **Personal access tokens** page

### Rotating the `RLADIESGUIDE` build hook

1. Log in at `app.netlify.com` and click **Sites** in the top nav
2. Click on the `guide.rladies.org` site
3. **Site configuration → Build & deploy → Build hooks**
4. Find the existing hook, click the **⋯** menu → **Regenerate URL**, copy the new URL
5. Update the secret on the `.github` repo: `gh secret set RLADIESGUIDE --repo rladies/.github` and paste the new URL
6. Verify by manually dispatching the workflow on the `.github` repo that calls the hook, and watching the guide rebuild in **Sites → guide.rladies.org → Deploys**

### Rolling back to a previous deploy

The fastest recovery from a bad ship — a broken layout on the guide, a workflow that merged a regression — is to republish the last known good deploy without touching git.
Go to **Sites → \<site\> → Deploys**, scroll to the most recent deploy that was healthy, click the **⋯** menu on that row, and click **Publish deploy**.
Netlify swaps production to that build in seconds; no rebuild, no CI wait.
Then fix the underlying problem in git at a normal pace and let the next merge replace the rolled-back deploy.

### Locking the production deploy

If a bad change is on `main` and more bad changes might be queued behind it, lock the production deploy to stop Netlify auto-publishing the next build.
**Sites → \<site\> → Deploys → Production deploys → Lock to current deploy**.
While locked, new builds still run but do not go live until the lock is released (same menu, **Unlock auto publishing**).
Use this during incident response: roll back to a good deploy, lock the site, then unwind the bad commits in git without racing the auto-deploy.

### Adding a new site to the account

Rare, but: **Sites** in the top nav → **Add new site → Import an existing project → Deploy with GitHub** → pick the repo → set the build command and publish directory → **Deploy site**.
After the first build, rename the site under **Site configuration → General → Site details → Change site name** so the auto-generated URL is recognisable.

### Re-running a stuck preview deploy

**Sites → rladies-preview → Deploys → Trigger deploy → Deploy site**.
This rebuilds against the latest content on the site without waiting for CI to upload a fresh artefact.

### Pausing a site

If a site is being abused or burning through build minutes for no good reason: **Sites → \<site\> → Site configuration → General → Build & deploy → Stop builds**.
Builds stop until manually re-enabled, which keeps the rest of the account healthy while the underlying cause is investigated.

## Troubleshooting

**Preview build failed at the publish step with an auth error.**
The `NETLIFY_AUTH_TOKEN` has expired or been revoked.
Rotate it following the steps above and re-run the failed workflow.

**Guide hasn't updated after a merge to `rladiesguide` `main`.**
The `RLADIESGUIDE` build hook did not fire.
Check the workflow on the `.github` repo that calls it for a non-2xx response, then confirm the build hook URL still exists in the Netlify dashboard.
If the workflow ran cleanly but no deploy appeared, the hook URL itself was probably rotated out of sync — generate a new one and update the secret.

**Custom domain TLS expired.**
Netlify normally auto-renews Let's Encrypt certificates well before they expire.
If the renewal failed, the fix is **Domain management → HTTPS → Verify DNS configuration → Renew certificate**.
If the verify step itself fails, the problem is upstream in Cloudflare's DNS — fix that first, then come back and renew.

**"You have used 100% of your build minutes for this billing period."**
The Free tier caps monthly build minutes.
Hitting the cap usually means either a runaway PR triggering preview builds in a loop, or the guide build hook firing on every commit instead of on merge.
Identify the offending site in **Sites → \<site\> → Deploys**, pause its builds while diagnosing, and check the workflow on the calling side for the loop.

## Things to watch for

- **Build-minute trend.** Glance at **Team settings → Billing → Usage** once a month; a creeping baseline usually means a workflow is firing more often than intended
- **Collaborator drift.** Review the people listed under **Team settings → Members** when anyone leaves leadership and remove stale entries — the Free tier's cap means an unused seat is a real cost
- **Stale tokens.** Any `NETLIFY_AUTH_TOKEN` or build hook URL that has not been touched in over a year should be rotated even if nothing is obviously wrong; a token that has been quietly working for a year is a token whose blast radius you have stopped thinking about
- **The verification step is not optional.** Every rotation runbook above ends with "trigger a build and watch it go green" for a reason — a secret is not rotated until the next real build proves it
