---
title: "Working with the website"
menuTitle: "Working with the website"
weight: 88
---

This is the practical, "how do I get a copy of the site running on my laptop and submit a change" guide.
It is written for someone who has used Git lightly — pushed a commit before, opened a pull request before — and who is comfortable enough on the command line not to be derailed by it.
If that is not you yet, [Happy Git with R](https://happygitwithr.com/) is the gentler ramp.

## What you actually need installed

[Hugo Extended](https://gohugo.io/installation/), version 0.144 or newer.
That is it.
You do not need Node, npm, R, or `renv` to preview the site or work on most content.
The repo's GitHub Actions handle the data-pipeline R scripts; the theme ships with its CSS and JavaScript already built.

If you are on macOS, `brew install hugo` works.
On Linux, the [GitHub Releases](https://github.com/gohugoio/hugo/releases) page has prebuilt binaries.
On Windows, [Scoop](https://scoop.sh/) (`scoop install hugo-extended`) is the path of least resistance.
Verify with `hugo version` — you want a line that says `extended` and a version `≥ 0.144`.

## Forking versus working directly on the repo

If you are a member of the [@rladies/website](https://github.com/orgs/rladies/teams/website) team, you have write access and can push branches directly to [rladies/rladies.github.io](https://github.com/rladies/rladies.github.io).
That is the recommended workflow for team members because preview deploys then have access to repository secrets and produce a hosted preview URL.

If you are not on the team, you fork the repo, push to your fork, and open a PR from your fork.
Preview deploys for fork PRs are gated — a team member dispatches the preview manually after a first review pass.
The build still runs against every fork PR, so you will know if the change breaks the site even without a hosted preview.

## Getting the code

The fastest way, if you have R and the [`usethis`](https://usethis.r-lib.org/) package installed:

```r
usethis::create_from_github("rladies/rladies.github.io")
```

That clones the repo (or forks then clones, if you do not have write access) into a sibling of your current project directory and opens it in your editor.

The terminal-only path:

```bash
git clone https://github.com/rladies/rladies.github.io.git
cd rladies.github.io
```

If you forked first, clone your fork instead:

```bash
git clone https://github.com/your-username/rladies.github.io.git
cd rladies.github.io
git remote add upstream https://github.com/rladies/rladies.github.io.git
```

The `upstream` remote is what lets you pull changes from the canonical repo into your fork later.

## Previewing the site locally

From the repository root:

```bash
hugo server
```

Hugo prints something like `http://localhost:1313/` once it is ready.
Open that.
Edits to content and templates trigger an automatic rebuild and the open page reloads.
A typical edit-rebuild cycle is well under a second.

The local build picks up the `development` config (relative URLs, drafts visible).
You will see only the small set of sample directory entries committed to the repo, not the full directory — that is by design.
The chapter map, events calendar, FAQ, dark-mode toggle, and most of the chrome work locally without needing the directory data.

## Making a change

Create a feature branch.
The branch name does not matter much, but a short verb-y description is kind to reviewers:

```bash
git checkout -b add-spanish-faq
```

Make your edits.
Save.
Watch the local preview update.
When happy, stage and commit:

```bash
git status
git add content/path/to/the/file.md
git commit -m "FAQ: add Spanish translation for build-system question"
```

Push the branch:

```bash
git push origin add-spanish-faq
```

Then open a PR through the GitHub UI — GitHub usually shows a "Compare & pull request" banner once you push a new branch.
If you used `usethis::pr_push()` from R, it will offer to open the PR for you.

The PR template asks for a short summary.
Be specific about what changed and why.
"Update FAQ" is less helpful than "Add Spanish FAQ entry covering the move from blogdown to plain Hugo".

## What happens after you open the PR

The CI runs.
You will see green checks and red Xs as workflows complete:

- **Production build check** — runs the same build the live site uses. Most failures here are template syntax errors or a remote data fetch timing out.  
- **JSON validation** — only fails if you touched a chapter or directory JSON file. The comment will tell you which field.  
- **Blog post lint** — only runs on changes under `content/blog/` or `content/news/`. Fails on missing alt text, missing required front matter, or an unparseable date.  
- **i18n coverage** — flags missing translation files and i18n keys without failing the build.  
- **Lighthouse + bundle size + broken links** — three reports posted as comments. Fails the PR only if an image has no alt text.  

If a check is red and the comment is not enough to figure out the fix, tag a team member or post in `#team-website`.

A team member is auto-assigned as your reviewer.
Expect feedback through GitHub code review — they may approve, request changes, or leave specific suggestions.
Apply the suggestions on your own branch and push again; the PR updates automatically.

## Keeping your fork in sync

If you are working in your own fork over a longer period, you will eventually fall behind `main`.
Pull from upstream, then push to your fork:

```bash
git checkout main
git fetch upstream
git merge upstream/main
git push origin main
```

Or with `usethis`:

```r
usethis::pr_pull()
usethis::pr_push()
```

## When something is wrong with your local build

Most "Hugo says my page is missing" problems come down to file naming.
The English version of a page is `index.en.md`, not `index.md`.
The section index is `_index.en.md`, not `index.en.md`.
A file named correctly but in the wrong directory does not appear at the URL you expected.

If you cannot get Hugo to start at all, check the version: `hugo version`.
If you are below 0.144, upgrade.

If the build runs but a feature is silently broken (a card has no image, a filter shows no options), inspect the page in the browser dev tools and look at what data the template tried to read.
Almost always the answer is in the front matter or in a missing data file.

## Asking for help

The fastest way to get help is to push your branch, open a draft PR, and link it in `#team-website` with a one-line description of what is stuck.
"My Hugo build fails on a directory page" is much easier for a maintainer to look at than the same description without the branch.
