---
title: "Creating a pretty URL"
menuTitle: "Redirects (pretty url)"
weight: 9
---

The site supports "redirect" pages — content files that exist only to bounce visitors from a clean URL on `rladies.org` to somewhere else.
This is how `rladies.org/form/blog-post` becomes a long Airtable form URL, how `rladies.org/positconf24` jumps to a Slido during a conference, how the directory update form keeps working under three different historical paths.

The pattern is small and self-contained, but it solves a real problem.
Forms move.
URLs change.
Slack messages, printed flyers, conference slides, and other community sites linking to us are everywhere we cannot edit retroactively.
A redirect on our own site is the one URL we always control — change it once, every old reference still works.

## How a redirect page is structured

A redirect is a markdown file with front matter and no body:

```yaml
---
title: "RLadies+ Posit::Conf 2024 meetup"
type: redirect
redirect: https://app.sli.do/event/some-event-id
---
```

Three things matter.
`type: redirect` tells Hugo to use [`themes/hugo-rladiesplus/layouts/redirect/single.html`](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/layouts/redirect/single.html) instead of the default single-page layout.
`redirect:` is the destination URL.
`title:` shows up briefly while the redirect page is animating, and in the title bar.

The layout shows a small RLadies+ submark, a one-line "Redirecting…" message, and a fallback button.
Two seconds later the page calls `window.location.replace(...)` to jump to the destination.
Any query string on the original URL is appended to the destination — the redirect at `rladies.org/form/blog-post?prefill_name=Mo` ends up at `<airtable-url>?prefill_name=Mo`.

A redirect does not need to be permanent.
If the Posit::Conf redirect from 2023 is no longer useful, delete the file.
The URL goes 404, which is fine.

## Where redirect files go

Redirects to forms live under `content/form/`.
The convention is one file per form, named for the form: `content/form/blog-post.md` produces `rladies.org/form/blog-post`.

Other one-off redirects (a conference link, a campaign URL) can sit at the content root: `content/positconf24.md` produces `rladies.org/positconf24`.
If you are not sure where a redirect belongs, open an issue describing what you want, and the team will help you pick a location.

## Aliases for legacy URLs

Hugo's [aliases](https://gohugo.io/content-management/urls/#aliases) feature lets a single page also resolve from other URLs.
The directory update form is a good example — the form has lived under three different paths over its lifetime, and we never want to break the old paths:

```yaml
---
title: "RLadies+ Directory form"
type: redirect
redirect: https://airtable.com/appzYxePUruG9Nwyg/pagF4TCWTbkjfuyLn/form
aliases:
  - /directory/update/
  - /directory-update/
  - /directory-update
---
```

Each alias is a URL Hugo will also generate, each pointing at the same redirect target.
A printed flyer that says "Update your entry: rladies.org/directory-update" still works even if the canonical path is now `rladies.org/form/directory-update`.

## How to create a new redirect

The repository is set up so this is a small task.

1. Create a feature branch.  
2. Copy the [redirect archetype](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladiesplus/archetypes/redirect.md) into the right location — for example, `content/form/awesome-thing.md` for `rladies.org/form/awesome-thing`.  
3. Fill in the `title`, `redirect`, and (if needed) `aliases`.  
4. Optionally run `hugo server` and check that visiting the path bounces to the destination.  
5. Open a PR, wait for the preview build, and confirm the redirect on the preview URL.  
6. Once merged, share the `rladies.org/form/awesome-thing` URL — never the underlying Airtable or Slido URL.  

The Hugo command line can also create the file for you:

```bash
hugo new --kind redirect form/awesome-thing.md
```

That uses the redirect archetype from the theme, so the front matter scaffolding is correct from the start.

## Why we never share the underlying URL

Some forms have a literal URL that looks like `https://airtable.com/appzYxePUruG9Nwyg/pagF4TCWTbkjfuyLn/form` — opaque, brittle, easy to break the next time the form is duplicated.
A few minutes after the form is moved or rebuilt, the underlying URL changes.
Every place we shared the old URL is now broken.

`rladies.org/form/awesome-thing` does not break.
The URL is the contract; the destination is the implementation.
This is a small thing that has paid for itself many times.
