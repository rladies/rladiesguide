---
title: "Troubleshooting"
menuTitle: "Troubleshooting"
weight: 70
---

A few failure modes that have actually happened.

## GitHub Actions

**"A JSON web token could not be decoded"** -- the private key in `JINX_PRIVATE_KEY` is not a valid PEM.
This usually means the contents were truncated when pasted, or a repo-level secret is shadowing the org-level one.
Check `gh secret list --repo rladies/<repo>` for repo-level overrides and delete them if they exist.

**"Resource not accessible by integration"** -- the token was minted, but the action is outside Jinx's permission set on the target repo.
Either Jinx is not installed on the target, or the permission needed is not yet granted.
Check the app installation list and the permission table in [For developers]({{< relref "for-developers" >}}).

## Slack

**"channel_not_found" in Slack** -- the bot needs to be in the channel to post.
For DMs, open a conversation with the Jinx app first.
For private channels, `/invite @Jinx`.
Public channels work if the bot has `chat:write.public` scope.

**Slack says "app did not respond"** -- the Cloudflare Worker is down or the signing secret is wrong.
Check the worker logs with `npx wrangler tail` from the `jinx` repo.

**No response from Jinx in Slack after the quip** -- the GitHub workflow failed.
Check the [Jinx Actions runs](https://github.com/rladies/jinx/actions) for `repository_dispatch` events.
If the workflow fails, Jinx tries to post the error back to Slack with a link to the logs.

**Jinx is silent in a workspace** -- the worker's allowlist (`SLACK_ORGANIZER_TEAM_ID` / `SLACK_COMMUNITY_TEAM_ID`) probably does not include that workspace's team ID.
This is intentional: Jinx only responds in the two RLadies+ workspaces.

## RAG / DM answers

**"My whiskers came up empty on that one"** -- the RAG retrieval returned nothing above the relevance threshold for a DM/Assistant question.
The indexer probably has not seen the topic yet (it runs weekly).
Trigger `Bot · Index Content` manually with `gh workflow run "Bot · Index Content" --repo rladies/jinx`, or check whether the page it should have matched even exists in the guide/website.

**Jinx replies but the link goes nowhere** -- shouldn't happen; the worker strips any URL the model invents that wasn't in the retrieved sources.
If it does, file an issue with the question and the retrieved chunks (`/jinx feedback` will surface the most recent answers and their reactions).

## Airtable invites

**Webhook returns `403 Base not in Airtable token scope`** -- the Airtable PAT does not have access to the base the webhook is coming from.
Grant the PAT access in Airtable's settings; no worker redeploy is needed.

**Invite button does nothing** -- the worker logs (`npx wrangler tail`) will show the actual Slack API error.
The most common one is the bot missing `admin.users:write` on the community workspace.
