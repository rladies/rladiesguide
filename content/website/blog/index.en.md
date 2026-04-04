---
title: "Contribute to the RLadies+ Blog"
menuTitle: "Blog"
weight: 2
aliases:
  - /comm/blog
---

The RLadies+ blog runs on Hugo and accepts posts as plain markdown files.
You write in a folder, add your images alongside it, and open a pull request.
The rest of this page walks you through each step — from proposing a post to getting it published.

A more general guide to forking, cloning, and PR-ing the website lives in [its own chapter](/comm/website/fork-clone-pr).

## Propose a post

You _can_ submit a post by opening a PR directly, but we prefer to plan it with you first.
Submit a [post proposal](https://rladies.org/form/blog-post) and a blog editor will reach out to set a timeline.
This also means we can offer alternative workflows if the Git/GitHub route is not your preference.

Once a proposal is accepted, start drafting by following the steps below.

## Clone the project

Pick whichever Git workflow you are comfortable with — terminal, `{usethis}`, GitHub Desktop, the GitHub web UI.
We document two approaches here.
Garrick Aden-Buie's [Pull Request Flow with usethis](https://www.garrickadenbuie.com/blog/pull-request-flow-usethis/?interactive=1&steps=) is a thorough walkthrough of the `{usethis}` path.

### Via git in the terminal

```sh
git clone https://github.com/rladies/rladies.github.io.git rladies_website
cd rladies_website/
git checkout -b my_branch
```

### Via {usethis}

```r
usethis::create_from_github("rladies/rladies.github.io")
usethis::pr_init("my_branch")
```

## Write a new blog post

### Create the post folder

Posts live in `content/blog/` organised by year, then date-slug:

```
content/blog/YYYY/MM-DD-your-post-slug/
```

For example: `content/blog/2026/04-02-community-survey-results/`.

Inside that folder, create your post file as `index.en.md`.
Writing in another language?
Use the appropriate suffix — `index.es.md` (Spanish), `index.pt.md` (Portuguese), or `index.fr.md` (French).

Place images and any other assets directly in the same folder.

### Front matter

Every post starts with a YAML block.
Here is a complete example:

```yaml
---
title: "Your post title"
author:
  - name: "Your Name"
    directory_id: "your-directory-id"
    url: "https://your-website.com"
editorial:
  - name: "Editor Name"
    directory_id: "editor-directory-id"
date: "2026-04-02"
description: |
  A short summary of your post. Markdown supported.
image:
  path: "featured-image.png"
  alt: "Descriptive alt text for the image"
slug: "your-post-slug"
tags:
  - community
  - survey
categories:
  - tutorials
---
```

What each field does:

- **title** — shown in the post listing and the post page.  
- **author** — a list of authors. See [Authors and contributions](#authors-and-contributions) for details.  
- **editorial** — editors who reviewed the post. Same format as `author`.  
- **date** — publication date in `YYYY-MM-DD` format. Must be static — never use `r Sys.Date()` or similar.  
- **description** — a short summary for the listing page and SEO. Supports markdown. Use `|` for multiline text.  
- **image.path** — filename of a featured image in your post folder. Automatically resized and converted to webp.  
- **image.alt** — alt text for the featured image. Always include this.  
- **slug** — custom URL slug. Auto-generated from the title if omitted.  
- **tags** — topic tags. 4–5 is a good number. Browse existing [tags](https://rladies.org/tags/).  
- **categories** — broader groupings. Browse existing [categories](https://rladies.org/categories/).  

### Authors and contributions

Each author, editor, or translator entry accepts these fields:

- **name** (required) — the person's name.  
- **directory_id** (optional) — their RLadies+ directory profile ID, which creates a link to `rladies.org/directory/<id>/`.  
- **url** (optional) — an external URL. Used when there is no `directory_id`.  

Multi-author posts can track who did what with the _contributions_ system.
Define a legend of contribution codes in the front matter, then assign codes to each person:

```yaml
author:
  - name: Alice
    directory_id: "alice"
    contributions: [a, b]
  - name: Bob
    directory_id: "bob"
    contributions: [a, c]
editorial:
  - name: Carol
    directory_id: "carol"
    contributions: [d]
contributions:
  a: "Wrote the original post"
  b: "Created visualizations"
  c: "Collected survey data"
  d: "Edited for publication"
```

Superscript letters (ᵃ, ᵇ, ᶜ, ᵈ) appear next to each name, with a legend below the author list.

### Cross-posting

If your post originally appeared on another blog, add a `crosspost` field:

```yaml
crosspost:
  community: "Your Blog Name"
  url: "https://your-blog.com/original-post"
```

A notice with a link to the original appears at the top of the post.

### Translations

Create a new file in the same folder with the matching language suffix.
The English version is `index.en.md`; a Portuguese translation would be `index.pt.md`.

The site supports English (en), Spanish (es), Portuguese (pt), and French (fr).

Credit translators in the front matter:

```yaml
translator:
  - name: "Translator Name"
    directory_id: "translator-directory-id"
```

## Formatting your post

The site theme handles all visual styling — your job is to provide well-structured markdown.

The [Content Features Reference](/website/content-features/) documents everything available to you: images with captions, callout boxes, buttons, mermaid diagrams, and more.
It also covers common formatting pitfalls that come up in review (like repeating the title or bolding headings).

A few highlights for blog posts specifically:

- **Images** — always include alt text. Use a caption (in quotes after the path) to explain why the image matters. For the post's _featured_ image, use the front matter `image` field, not the body.
- **Callouts** — use `{{</* callout type="tip" */>}}` for tips, warnings, and notes.
- **Mermaid diagrams** — use fenced code blocks with the `mermaid` language tag.
- **Keep the body clean** — Hugo generates preview cards from the first lines of your post. Don't put metadata or author lists at the top.

## Preview locally

Run Hugo from the repository root:

```sh
hugo server -D
```

The `-D` flag renders draft posts so you can see yours before it is published.

## Submit your post

When the post looks right locally, push it for review.

### Via git in the terminal

```sh
git add content/blog/YYYY/MM-DD-your-post-slug
git commit -m "add post: your post title"
git push --set-upstream origin my_branch
```

### Via {usethis}

Stage and commit your post folder in the RStudio Git pane, then:

```r
usethis::pr_push()
```

## Open a pull request

Open a [pull request](https://github.com/rladies/rladies.github.io/pulls) against the `main` branch.
If you are not on the RLadies+ Global team, you will be working from a fork — the site build will run, but no deploy preview is generated.

A blog or website team member will be assigned as your reviewer and guide you through any remaining changes via GitHub code review.

### What happens after review

Once the team is happy with the post, they [create a branch](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-and-deleting-branches-within-your-repository#creating-a-branch) from the main repository (usually named `fork_[postname]`) and [switch your PR's base](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/changing-the-base-branch-of-a-pull-request) to that branch.
Your PR is merged there, a deploy preview is generated, and any final editorial tweaks happen on the team's side.
They will reach out if they need anything else from you.

## Questions

If anything here is unclear, open [an issue](https://github.com/rladies/rladiesguide/issues) and we will sort it out.
