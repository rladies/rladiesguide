---
title: "Commands"
menuTitle: "Commands"
weight: 10
---

The same `/jinx` commands work on GitHub and Slack.
The difference is where the response goes -- a GitHub issue comment or a Slack message.

The canonical command list lives at [`/jinx help`](https://github.com/rladies/jinx/blob/main/inst/commands/help.md) and is what Jinx shows when you ask `/jinx help`.

## Information commands

These commands return the information directly. No GitHub issues are created.

| Command                                     | What it does                                                |
| ------------------------------------------- | ----------------------------------------------------------- |
| `/jinx help`                                | Show all available commands                                 |
| `/jinx report weekly`                       | Org-wide activity summary (commits, PRs, issues)            |
| `/jinx report monthly`                      | Same, for the past 30 days                                  |
| `/jinx report chapters`                     | Chapter health: active vs inactive, months since last event |
| `/jinx gha-dashboard`                       | GitHub Actions CI status across all repos                   |
| `/jinx analytics`                           | Org-wide contributor and commit trends                      |
| `/jinx generate website analytics [period]` | Plausible website stats (7d/30d/month/6mo/12mo)             |
| `/jinx contributors [repo]`                 | Contributor list for a repo                                 |
| `/jinx contributors org`                    | Top contributors org-wide                                   |
| `/jinx events <chapter>`                    | Recent events for a chapter                                 |
| `/jinx cfp list`                            | Open calls for proposals                                    |
| `/jinx translate status`                    | Translation coverage across languages                       |
| `/jinx translate validate [lang]`           | Check translation placeholders                              |
| `/jinx chapter-health`                      | Check chapter activity health                               |
| `/jinx validate-directory`                  | Validate directory entries in a PR                          |
| `/jinx blog-check-links`                    | Check all blog URLs for broken links                        |

## Action commands

These commands perform an action (create an issue, send an invite, post an announcement) and reply with a link to what was created.

| Command                                 | What it does                                                  |
| --------------------------------------- | ------------------------------------------------------------- |
| `/jinx invite @user to <team>`          | Invite a user to the org and a team, creates onboarding issue |
| `/jinx offboard @user from <team>`      | Start offboarding, creates tracking issue                     |
| `/jinx chapter-setup <city> <country>`  | Create a chapter setup checklist issue                        |
| `/jinx chapter-update <city> <country>` | Create a chapter update issue                                 |
| `/jinx blog-add <url>`                  | Auto-create a blog entry from a URL                           |
| `/jinx cfp add <conf> <deadline> <url>` | Track a new call for proposals                                |
| `/jinx cfp recommend <conf> @speaker`   | Recommend a speaker for a conference                          |
| `/jinx contributors update [repo]`      | Update the contributors list via PR                           |
| `/jinx events sync`                     | Sync and publish the chapter event summary                    |
| `/jinx announce <post-url>`             | Announce a blog post on social media                          |
| `/jinx remind stale`                    | Nudge stale onboarding/offboarding issues, report links       |
| `/jinx slack-invite <email>`            | Post a Slack invite request for an organiser to action        |

## Slack-only commands

These four commands never leave the Cloudflare Worker -- they hit the Slack Web API directly, so they reply in seconds without needing a GitHub Actions run.
They exist only on Slack; running them in a GitHub comment will not do anything.

| Command                            | What it does                                                        |
| ---------------------------------- | ------------------------------------------------------------------- |
| `/jinx setup-channel`              | Pin the standard RLadies+ resource bookmarks in the current channel |
| `/jinx pair @alice @bob [message]` | Open a group DM with mentioned users (up to 7)                      |
| `/jinx remind-me <when> \| <what>` | Set a personal Slack reminder for yourself                          |
| `/jinx feedback [days]`            | Show reaction signal on Jinx's recent answers                       |

For how slash commands actually flow through the worker and GitHub Actions, see [Slash commands]({{< relref "slash-commands" >}}).
