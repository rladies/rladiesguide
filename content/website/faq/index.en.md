---
title: "Frequently asked questions"
menuTitle: "FAQ"
weight: 99
---

## Why is the site plain Hugo and not blogdown / Quarto?

Three reasons.

The site is mostly not "rendered content".
The majority of updates are to data (chapters, directory, events) or to plain prose pages — nothing that needs an R kernel to produce.
The blog is the one place where authors might want a computational document, and the convention there is to commit the rendered `.md` next to any `.qmd`/`.Rmd` source so Hugo only sees the markdown.
Old posts never need to be re-rendered.

Hugo on its own is faster and more stable.
Adding blogdown or Quarto on top of Hugo introduces another tool whose breaking changes can take down the build.
Plain Hugo means a clone with only Hugo installed produces the same output as the production build.
That keeps the contributor barrier low and the failure modes shallow.

Hugo's multilingual model is more capable than blogdown's.
The site supports four languages (English, Spanish, Portuguese, French), with placeholder generation for un-translated pages, hreflang alternates, per-language menus, and a config directory split by language and environment.
That depth is a first-class Hugo feature; layering blogdown on top would mostly get in the way.

## Do I need to install Node or npm?

Only if you are changing the theme's CSS, updating a vendor JavaScript library, or adding a new front-end dependency.
Day-to-day editing — content, data, translations — needs only Hugo Extended ≥ 0.144 and Git.
The reason and full workflow are in [Theme assets and npm bundling](/website/admin_guide/assets/).

## Do I need R installed?

Only if you are running the data-pipeline scripts locally (fetching the directory, filling in placeholder translations, validating JSON schemas).
For everyday content and template work, Hugo on its own is enough.
The GitHub Actions handle the R steps in CI, so most contributors never need an R install.

## Where does the directory data come from?

A separate, more access-controlled repo: [rladies/directory](https://github.com/rladies/directory).
The website repo only contains a few sample entries (`data/directory/sample-*.json`) so local previews work.
The full directory is cloned in at build time by the GitHub Actions and never lives in the public website repo.
The full story is in [The directory: where members live and how the site uses them](/website/admin_guide/directory-pipeline/).

## Why are some pages in non-English languages showing an "auto-translated" banner?

Because they were filled in by the [`scripts/missing_translations.R`](https://github.com/rladies/rladies.github.io/blob/main/scripts/missing_translations.R) script at build time.
The site supports four languages, and the script ensures every URL exists in every active language by copying the English page across with `translated: auto` in the front matter.
A reviewed translation has `translated: true` (or simply no `translated` field) and the banner disappears.

## How do I schedule a blog post for a specific date?

Set the `date` in the post's front matter to the desired publication date and label the PR `pending`.
The [`merge-pending.yaml`](https://github.com/rladies/rladies.github.io/blob/main/.github/workflows/merge-pending.yaml) workflow runs every weekday at 10:58 UTC, finds PRs whose front-matter date matches today, and squash-merges them automatically.
Production deploys from `main`, so the post appears on the site within minutes.

## A change went into `main` — when does it show up on rladies.org?

The production build runs on every push to `main` and twice a day on cron (`45 */12 * * *`).
The push-triggered build typically completes in five to ten minutes.
If you do not see your change after that, check the [Actions tab](https://github.com/rladies/rladies.github.io/actions) — the production build may have failed for unrelated reasons (a remote data fetch timing out, a directory entry validation problem) and just need a re-run.

## Why do PRs from forks not get a deploy preview?

Because preview deploys need access to private repository secrets — the directory deploy key, the Netlify auth token — and GitHub does not give those to PRs from forks for security reasons.
A maintainer can dispatch the preview workflow manually for a fork PR once they have reviewed the change.
The build itself still runs on every fork PR via [`check-build.yaml`](https://github.com/rladies/rladies.github.io/blob/main/.github/workflows/check-build.yaml), so build failures are still caught early.

## Where can I report a bug, suggest a feature, or ask a question?

[GitHub Issues](https://github.com/rladies/rladies.github.io/issues) for anything trackable.
`#team-website` in the [RLadies+ Slack](https://r-ladies-community.slack.com/) for quick informal questions.
