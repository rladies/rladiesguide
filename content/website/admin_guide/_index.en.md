---
title: Website Admin Guide
weight: 99
chapter: true
menuTitle: Website Admin Guide
---

Maintaining the RLadies+ site is part editorial work, part low-key sysadmin.
Tasks range from restructuring whole sections of the site, to reviewing incoming pull requests, to debugging a directory entry that did not show up where the contributor expected.
The pages under this section are the working knowledge of the website team — the things you will reach for when something is broken, when somebody asks "where does X live", or when you onboard a new team member.

If you have never worked on the site before, read [How the site is built](/website/admin_guide/hugo/) first, then [The directory: where members live and how the site uses them](/website/admin_guide/directory-pipeline/).
Those two together give you the spine of what this site is and what makes it tick.

## The website team

The website team is a group of RLadies+ global members responsible for maintaining the website.
That covers reviewing pull requests, keeping the site up to date and functional, managing content (adding pages, updating existing ones, fixing accessibility regressions), and handling the integrations with the directory, the meetup archive, and Airtable.

The team has admin access to the [rladies/rladies.github.io](https://github.com/rladies/rladies.github.io) repository, which means we work directly on branches in the repo rather than forks.
External contributors fork; we do not.
This keeps the review history simpler and avoids the fork-sync overhead — preview deploys themselves work the same either way.

## How we communicate

`#team-website` in the [RLadies+ Slack](https://r-ladies-community.slack.com/) is where day-to-day discussion happens — quick questions, "is anyone online to look at this", informal coordination.
Official tasks, decisions, and any work that needs to be discoverable in three months go in GitHub issues.
If a Slack thread leads anywhere, link it from the issue.

## GitHub setup

The `main` branch is protected.
Nobody can push directly.
Every change goes through a pull request, every pull request needs at least one approving review from another team member, and every required check must pass before merge.
If no other team member is available to review and the change is urgent, ask leadership.

We use GitHub Issues for tracking work, GitHub PRs for changes, and GitHub Actions for everything else — builds, deploys, lints, scheduled syncs.
The full action lineup is documented at [GitHub Actions and CI](/website/admin_guide/gha/).

## Codeowners

The repository has a [CODEOWNERS](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners) file that auto-assigns reviewers based on which paths a PR touches.
The website team owns the site overall, but specific paths are owned by the working groups that maintain that content.
Other reviewers are welcome on any PR, and the second-pair-of-eyes rule still applies.

| Path                                | Team                                                                |
| ----------------------------------- | ------------------------------------------------------------------- |
| `/content/`                         | [@rladies/translation](https://github.com/orgs/rladies/teams/translation) |
| `/i18n/`                            | [@rladies/translation](https://github.com/orgs/rladies/teams/translation) |
| `/content/news`                     | [@rladies/global](https://github.com/orgs/rladies/teams/global) and [@rladies/leadership](https://github.com/orgs/rladies/teams/leadership) |
| `/content/blog`                     | [@rladies/blog](https://github.com/orgs/rladies/teams/blog)         |
| `/content/coc`                     | [@rladies/coc](https://github.com/orgs/rladies/teams/coc)           |
| `/content/directory`                | [@rladies/directory](https://github.com/orgs/rladies/teams/directory) |
| `/content/activities/mentoring`     | [@rladies/mentoring](https://github.com/orgs/rladies/teams/mentoring) |

## Handling pull requests

The website has GitHub branch protection rules so a stray push cannot break the site.
There are also GitHub Actions that run checks on every PR — the build itself, JSON validation, blog frontmatter linting, i18n coverage, Lighthouse, broken-link checks.
All PRs, including ones from website team members, get reviewed before merge.

The general rules for merging:

- All required checks must be green. If a check is failing and you do not know how to help the PR author resolve it, tag another team member or ask in `#team-website`.  
- Always review the actual changes through a [GitHub code review](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/about-pull-request-reviews) — Approve, Request changes, or Comment as appropriate.  
- Whoever approves can also merge. Default to a rebase merge; squash is fine when a contributor specifically requests it (e.g., a clean blog-post commit).  

Every PR is auto-assigned to a website team member.
That assignee is the first owner of the review.
If you cannot finish a review you have been assigned — because the change is in code you are not familiar with, or because you are unavailable — it is your job to find another reviewer.
When in doubt, get a second opinion.

The full reviewer's walkthrough — how to read each GHA comment, what to look for by change type, how to tell a flake from a real failure, when to approve versus request changes — lives at [Reviewing pull requests](/website/admin_guide/review-prs/).

## Setting up locally

You only need [Hugo Extended](https://gohugo.io/installation/) ≥ 0.144 and Git for everyday work.
The R toolchain (`renv`, the scripts under `scripts/`) is only needed if you are working on the data pipeline.
Full setup details are at [Working with the website](/website/fork-clone-pr/).

The repo also ships with a project `.Rprofile`.
Do not edit it.
If you want personal R settings, add a separate `~/.Rprofile` — the project file will load alongside it.

## What lives where in this guide

The pages in this section, in the order you will probably read them:

- [How the site is built](/website/admin_guide/hugo/) — the mental model: theme, config, content, layouts, render hooks, shortcodes, data, multilingual.  
- [Theme assets and npm bundling](/website/admin_guide/assets/) — Tailwind v4, esbuild, Font Awesome, fonts, and the contract that lets contributors avoid Node.  
- [The directory: where members live and how the site uses them](/website/admin_guide/directory-pipeline/) — the directory data flow and why it threads through every other section.  
- [A tour of the sections](/website/admin_guide/sections/) — what each section does, where its layout lives, and what feeds it.  
- [Shortcodes, callouts, and render hooks](/website/admin_guide/shortcodes/) — the markdown features that go beyond plain markdown.  
- [Dark mode and theming](/website/admin_guide/dark-mode/) — how the toggle works, how to add a component, brand tokens.  
- [SEO, social previews, and accessibility](/website/admin_guide/seo/) — what gets generated automatically, how to fix a broken Slack preview, what the Lighthouse audit enforces.  
- [GitHub Actions and CI](/website/admin_guide/gha/) — every workflow, what it checks, what to do when one fails.  
- [Reviewing pull requests](/website/admin_guide/review-prs/) — reading GHA comments, what to look for by change type, when to approve, when to merge.  
- [Blog Administration](/website/admin_guide/blog/) — Airtable workflow for the blog editorial team.  
- [Creating a pretty URL](/website/admin_guide/redirects/) — `rladies.org/form/<thing>` and how those redirects are authored.  
