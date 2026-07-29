---
title: "Contribute to the RLadies+ Blog"
menuTitle: "Blog"
weight: 2
aliases:
  - /comm/blog
---

The RLadies+ blog is where members write down what they have built, learned, organised, or noticed.
Posts come from chapter organisers, mentors, package authors, conference speakers, anyone in the community who has something worth sharing.
We publish in markdown, on the [main RLadies+ website](https://rladies.org/blog/), through pull requests on the [website repository](https://github.com/rladies/rladies.github.io).

This page walks through everything from "I have an idea for a post" to "the post is live on the site".
The companion guide [Working with the website](/website/fork-clone-pr/) covers the general fork-clone-PR mechanics if Git is new to you.

## Step 1: Propose a post

You can submit a draft directly as a PR, but we strongly prefer that you propose first.
[Submit a post proposal](https://rladies.org/form/blog-post) and one of the blog editors will be in touch.

Why bother with the proposal step?
A few reasons.

The editorial team can plan the publication calendar so two posts are not competing for attention on the same week.
The editor can offer feedback on scope and shape before you spend hours writing.
If you are not comfortable with the Git/GitHub workflow, the editor can pair you with someone who is, or accept your draft as a Google Doc and convert it.
For a first-time contributor, this turns "open a PR with a polished post" into "send us your idea and we will help you get it published" — a much smaller step.

If you would rather just write and PR, that is fine too.
Skip to step 3.

## Step 2: Wait for an editor to confirm scope and timeline

The editor will reply with one of three things: accept and propose a publication date, accept with suggested scope changes, or decline with reasoning.
The blog team uses an [Airtable workflow](/website/admin_guide/blog/) to track proposals; you do not need to know how that works to use it.

Once the proposal is accepted you can start drafting.

## Step 3: Get a working copy of the website

The full mechanics are in [Working with the website](/website/fork-clone-pr/).
The short version, if you are comfortable with terminal Git:

```bash
git clone https://github.com/rladies/rladies.github.io.git
cd rladies.github.io
git checkout -b my-blog-post
hugo server
```

Or, with the `usethis` R package:

```r
usethis::create_from_github("rladies/rladies.github.io")
usethis::pr_init("my-blog-post")
```

A blog post is plain markdown, so the minimum requirement is just [Hugo Extended](https://gohugo.io/installation/) — no Node, no npm, no R needed.
That said, the site still works happily with R-side workflows.
If you write your posts in `.Rmd` or `.qmd`, the [blogdown](https://bookdown.org/yihui/blogdown/) and [hugodown](https://hugodown.r-lib.org/) packages remain valid ways to scaffold posts and run a live preview from RStudio.
Step 7 covers each authoring choice — plain markdown, blogdown, hugodown, and Quarto — and how to commit the rendered output.

## Step 4: Create the post folder

Each post is a [page bundle](https://gohugo.io/content-management/page-bundles/) under `content/blog/<year>/<slug>/`.
Pick a slug that is short, descriptive, and uses dashes:

```
content/blog/2026/06-iwd-recap/
└── index.en.md
```

The folder name should start with a date prefix in `MM-DD-` form so chronological sorting works in the directory listing.
Inside the folder, your post lives in `index.en.md` (English) — add `index.es.md`, `index.pt.md`, or `index.fr.md` files alongside if you have translations.

You can scaffold the file from the theme's archetype:

```bash
hugo new --kind blog blog/2026/06-iwd-recap/index.en.md
```

That gives you a starting point with the right front matter and a placeholder body.

## Step 5: Fill in the front matter

The block at the top between two `---` lines is YAML front matter — structured metadata.
Required, recommended, and optional fields:

```yaml
---
title: "Spelling and casing of the post title"
description: |
  One-line subtitle, used in OG meta and listings. Markdown supported.
author:
  - name: Your Name
    directory_id: "your-directory-slug"   # optional but encouraged
editorial:
  - name: Editor Name
    directory_id: "editor-directory-slug"
date: "2026-06-12"
categories: [community]
tags: [iwd, recap, oslo]
image:
  path: "feature-image.png"
  alt: "Description of the feature image, for screen readers"
summary: |
  Optional override for the listing summary.
---
```

The fields, plainly:

`title` — sentence case, no period at the end.

`description` — one or two sentences. This is what appears under the post title in listings and in the OpenGraph card on Slack/LinkedIn previews. If you skip it, the listing falls back to the auto-generated summary.

`author` — list of structured entries. Each entry has at least a `name` and, when the author has a [directory profile](https://rladies.org/directory/), a `directory_id`. When `directory_id` is present, the byline becomes a link to the profile page, where the post will also appear under "Posts by this author". This is how the site connects authors to everything else they have done.

`editorial` — same shape as `author`. Use it to credit the editor who reviewed the post.

`contributions` — see the section just below. Lets you spell out what each author and editor actually did, with a small superscript next to each name and a legend under the byline.

`date` — the publication date as `YYYY-MM-DD`. Hard-fail check: must literally be that format, must literally be a date. The blog-lint workflow will block the PR if it is not.

`categories` and `tags` — keywords. Categories are broader, tags are narrower. Click an existing category at the bottom of any post to see what conventions are in use.

`image.path` — the file name of the feature image, which lives next to `index.en.md` in the same folder. Always provide `image.alt` — the Lighthouse and blog-lint checks will fail the PR without it.

`summary` — only set this if the auto-generated summary (the first 30 words) does not work for the listing.

## Crediting who did what: per-author contribution notes

Posts often have several people behind them — one person wrote the bulk of the prose, another translated quotes from interviews, an editor reshaped the structure, a community lead organised the surveys the post draws on.
"Author" and "Editor" are coarse labels.
The site supports finer-grained credit through a small contributions system, modelled on the way academic papers list contribution roles.

You declare a `contributions:` map at the post level — keys are short letter codes, values are human-readable descriptions of what that role means.
Then on each author or editor entry, you list the contribution codes that apply to that person.
The byline shows each name with the matching superscripts (ᵃ, ᵇ, ᶜ…) and a legend underneath spelling out what each letter stands for.

A worked example:

```yaml
---
title: "How RLadies+ Santa Barbara built a city-wide R survey"
date: "2026-04-12"
author:
  - name: Beatriz Milz
    directory_id: "beatriz-milz"
    contributions: [a, b]
  - name: Haydee Svab
    directory_id: "haydee-svab"
    contributions: [a, c]
  - name: Tatyane Paz Dominguez
    contributions: [c]
editorial:
  - name: Athanasia Mo Mowinckel
    directory_id: "athanasia-mo-mowinckel"
    contributions: [d]
contributions:
  a: "Wrote the original post"
  b: "Organised community events"
  c: "Conducted the diversity survey"
  d: "Edited for publication"
---
```

What the reader sees in the byline:

- Beatriz Milzᵃᵇ
- Haydee Svabᵃᶜ
- Tatyane Paz Domínguezᶜ
- Athanasia Mo Mowinckelᵈ (under "Editor")

…and a small legend below: `ᵃ Wrote the original post · ᵇ Organised community events · ᶜ Conducted the diversity survey · ᵈ Edited for publication`.

A few rules of thumb for using this well:

Use single-letter lowercase keys (`a`, `b`, `c`, …).
The theme has a fixed mapping from those letters to Unicode superscripts; uppercase letters and multi-character keys will not render as superscripts.

Keep the descriptions short.
Each one becomes a chip in the legend below the byline; multi-clause sentences crowd the layout.
"Wrote the original post" is good. "Wrote the original post and contributed substantially to the editorial review process" is too long — split it into two roles or pick the dominant one.

A person can hold multiple contributions: `contributions: [a, b]` is fine.

If you do not declare a `contributions` map at the post level, the per-person `contributions: [...]` lists are silently ignored.
Both pieces are needed for the legend to render.

If you have only one author and the post is straightforward, skip the whole system.
Contributions exist for posts where the credit story matters — community surveys, multi-author retrospectives, translated content with a meaningful translator role.

The same `contributions` mechanism applies to entries under `translator:` if you want to distinguish, say, "translated the body" from "translated the code comments".
The translator block is rendered the same way as the author and editor blocks.

## Step 6: Write the post

The body is plain markdown.
The site uses [render hooks](/website/admin_guide/shortcodes/) so a few things behave differently from vanilla markdown:

A block-level image with a title becomes a captioned `<figure>`:

```markdown
![Group photo of attendees](photo.jpg "Attendees of the IWD 2026 Oslo meetup")
```

External links automatically get `target="_blank"` — you do not need to write the attribute by hand.

A fenced ```` ```mermaid ```` block renders as a Mermaid diagram.

The site provides three custom shortcodes (`button`, `callout`, `rlogo`) that are documented at [Shortcodes, callouts, and render hooks](/website/admin_guide/shortcodes/).

Authoring conventions on this site:

- One sentence per line. Diffs are kinder to reviewers when one changed sentence shows as one changed line.  
- Two trailing spaces at the end of a list item to ensure clean line breaks across markdown renderers.  
- Underscores for italics (`_italic_`), not asterisks.  
- Em dashes get spaces around them — like that.  
- Never bold a heading.  
- Always provide alt text on every image; use `alt=""` for purely decorative ones.  

Save and watch the local preview at `http://localhost:1313/blog/2026/06-iwd-recap/`.

## Step 7: Authoring with R or Quarto

If your post is a tutorial that needs to run R code, you can author it in a `.qmd` (Quarto) or `.Rmd` (R Markdown) file alongside `index.en.md`.
The convention is the same regardless of which engine you use: **render to `.md` locally and commit both files**.
Hugo only sees the `.md` — the source is committed for reproducibility, so reviewers and future contributors can re-knit it.

```
content/blog/2026/07-tidymodels-walkthrough/
├── index.en.qmd       # the source (or .Rmd)
├── index.en.md        # the rendered output, committed
├── chunks/            # any cached output
└── feature.png
```

The `ignoreFiles` config in `hugo.yaml` excludes `*.Rmd` and `*.qmd` from the build, so the source files sit alongside the rendered markdown without confusing Hugo.

Pick the engine you are most comfortable with — they all produce the same Hugo-ready markdown.

### Option A: blogdown

If you already use [blogdown](https://bookdown.org/yihui/blogdown/) day to day, it still works for this site.
You will use blogdown only as an authoring helper — `serve_site()` for live preview and the New Post addin for scaffolding — not as the build tool.

Recommended setup:

```r
install.packages("blogdown")
```

From the project root in RStudio, the blogdown "New Post" addin (`Addin → New Post`) will scaffold the page bundle and front matter for you.
Pick the `blog` archetype, fill in the title, set the date and tags, and choose `Rmd` as the format if you want code chunks.

For live preview, use:

```r
blogdown::serve_site()
```

When you knit the `.Rmd`, blogdown writes the `.md` next to it.
Commit both files.

A few site-specific notes:

- The site does not use `renv` for blog posts.
  Earlier versions of this guide insisted on `renv::restore()` — that is no longer needed unless you are working on the site infrastructure itself.
- The theme is `hugo-rladiesplus`, not the blogdown default themes.
  blogdown's "New Post" addin still works because it only writes to `content/`, but theme-specific scaffolds (like the home page) will not render correctly inside `serve_site()` unless you have Hugo Extended installed.

### Option B: hugodown

[hugodown](https://hugodown.r-lib.org/) is a leaner alternative to blogdown — it is intentionally just enough to render `.Rmd` to `.md` for a Hugo site.

```r
install.packages("hugodown")
```

To render a single post:

```r
hugodown::use_post("blog/2026/07-tidymodels-walkthrough/index.en.Rmd")
hugodown::render(input = "content/blog/2026/07-tidymodels-walkthrough/index.en.Rmd")
```

For live preview:

```r
hugodown::hugo_start()
```

The `_hugodown.yaml` shipped with the post controls front-matter conversion; the defaults work for this site.
Commit the `.Rmd` and the rendered `.md`.

### Option C: Quarto

If you author in Quarto, the only thing you need in your `.qmd` front matter is:

```yaml
---
title: "Your post title"
format: hugo-md
---
```

That is it.
The repository's root [`_quarto.yml`](https://github.com/rladies/rladies.github.io/blob/main/_quarto.yml) already wires in a small Lua filter, [`figure-to-markdown.lua`](https://github.com/rladies/rladies.github.io/blob/main/figure-to-markdown.lua), so any `.qmd` rendered anywhere under the repo picks it up automatically.
You do not need a `filters:` line, you do not need a relative path, you do not need to copy anything from another post.

Render with `quarto render path/to/index.en.qmd` and commit the resulting `.md` next to the source.

### How figures should be written

Whichever engine you pick, write code chunks that produce figures with **both** `fig-alt` and `fig-cap`.
The figures end up as plain markdown images (`![alt](src "caption")`) in the rendered `.md`, which the theme's [image render hook](/website/admin_guide/shortcodes/) turns into a `<figure>` with a `<figcaption>` and `loading="lazy"`.

````markdown
```{r}
#| label: scatter-age-score
#| fig-alt: "Scatterplot of age against score, points coloured by treatment group, showing a positive trend in both groups"
#| fig-cap: "Score increases with age in both treatment arms (n = 142)"
plot(...)
```
````

`fig-alt` is the descriptive alt text for screen readers — it should describe what the figure _shows_.
`fig-cap` is the visible caption under the figure — it should explain why the figure _matters_.
The two are different jobs and reading the post aloud should still make sense without seeing the figure.

If you only set `fig-cap` and skip `fig-alt`, the filter will use the caption as alt text too.
That is a workable fallback for figures whose caption is already descriptive enough to stand in for alt — but treating the caption as a "descriptive alt" is the exception, not the rule.
The default expectation is both fields, written for their distinct audiences.

The same applies to inline markdown images written by hand: `![descriptive alt](image.png "caption")` is the form the render hook expects.

## Step 8: Commit and push

```bash
git add content/blog/2026/06-iwd-recap
git commit -m "blog: IWD 2026 Oslo recap"
git push --set-upstream origin my-blog-post
```

Or with `usethis`:

```r
usethis::pr_push()
```

## Step 9: Open a PR

Open the PR against `main`.
GitHub will show a "Compare & pull request" prompt once you push.

A bot will post the editorial checklist as a PR comment.
The blog-lint, JSON validation, build, Lighthouse, and link-check actions will run automatically.
A team member from `@rladies/blog` or `@rladies/website` will be auto-assigned as your reviewer.

Expect feedback through GitHub code review.
You can apply suggestions directly from the GitHub UI ("commit suggestion") or pull them down and edit locally — both push back to your branch and update the PR.

## Step 10: Schedule publication

Once the post is approved and the editorial team picks a publication date, two things happen.
The PR gets the `pending` GitHub label.
The post's front-matter `date` is set to the publication date.

The [`merge-pending.yaml`](https://github.com/rladies/rladies.github.io/blob/main/.github/workflows/merge-pending.yaml) workflow runs every weekday at 10:58 UTC, finds PRs with the `pending` label whose front-matter date matches today, and merges them.
The production build runs immediately after; the post is live within ten minutes.

If the publication date passes without the PR being labelled `pending`, nothing happens — the workflow only acts on labelled PRs.
The team explicitly opts each post in.

## When something is unclear

The most common confusion is around `directory_id`.
If you do not have a directory profile yet, you can either submit one through [the directory form](https://rladies.org/form/directory-update) (and reference its ID once it is live) or omit `directory_id` for now and add it in a follow-up PR.
The byline will render as plain text without it; everything else still works.

If anything else is unclear, [open an issue on the guide repo](https://github.com/rladies/rladiesguide/issues).
The guide improves when contributors tell us where it is fuzzy.
