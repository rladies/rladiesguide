---
title: "Add a new mentoring entry"
menuTitle: "Add mentoring entry"
weight: 5
---

The mentoring page on the site, at <https://rladies.org/about-us/global-team/mentoring/>, surfaces stories of chapters that have run a mentorship and what they got out of it.
Each story is a small page bundle under `content/mentoring/`, with a quote from the mentee and a link to a longer blog post telling the full story.
Adding one is a small PR.

You will first need [your own local copy](/website/fork-clone-pr/) of the website.
You only need Hugo Extended installed — no R, no `blogdown`, no `renv`.

## Create the page bundle

Each mentoring entry lives in its own folder under `content/mentoring/`.
The folder name is the chapter slug (lowercase, dash-separated).
Inside, an `index.en.md` and the chapter's photo:

```
content/mentoring/cotonou/
├── index.en.md
└── cotonou.png
```

You can scaffold this with the theme's mentoring archetype:

```bash
hugo new --kind mentoring mentoring/cotonou/index.en.md
```

That creates a folder with a starter `index.en.md` containing the right front matter fields.

## Fill in the front matter

A complete entry looks like this:

```yaml
---
title: RLadies+ Cotonou
date: "2019-01-01"
mentee: "Nadeja Sero"
mentor: ""
image:
  path: "cotonou.png"
  alt: "Photograph of the Cotonou chapter mentoring session"
article: /blog/2021/02-04-cotonout_mentoring
summary: |
  I asked the speaker on that day to share her experience from choosing her
  topic to presenting! She ecstatically shared that and we immediately got
  volunteers for the next meetup.
---
```

Fields, plainly:

`title` — the chapter name as it should appear on the card.

`date` — the date of the mentoring story (`YYYY-MM-DD`).

`mentee` — name of the mentee. Required if the mentee is willing to be named.

`mentor` — name of the mentor. Often empty when the mentor was the chapter at large.

`image.path` — filename of the chapter photo, in the same folder. Always provide `image.alt`.

`article` — internal path to a longer blog post about the mentoring experience (`/blog/<year>/<slug>/`). Optional but encouraged — the listing turns this into a "Read more" link on the card.

`summary` — a quote from the mentee about the mentorship. This is what shows on the card. Use the mentee's actual words when possible.

The body of the file can stay empty — the mentoring layout uses only the front matter.
If you want to add a longer description that does not fit in `summary`, write it in the body and the layout will use it as the expanded content when a visitor hovers the card.

## Commit and PR

```bash
git add content/mentoring/cotonou
git commit -m "mentoring: add Cotonou entry"
git push --set-upstream origin add-cotonou-mentoring
```

Open the PR through the GitHub UI.
A team member will review and merge.

## A note on writing the linked blog post

The `article` field links to a longer story published as a blog post.
If the post does not yet exist, that is fine — leave the field blank and add it once the post is live.
Or write the blog post first (see [Contribute to the RLadies+ Blog](/website/blog/)) and add the mentoring entry once you have the URL.
The mentoring page works fine without the link; it just shows the quote and the chapter name.
