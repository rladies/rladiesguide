---
title: Website
weight: 7
chapter: true
slides: true
menuTitle: Website
---

This section is the working knowledge for maintaining the [RLadies+ global website](https://rladies.org) at <https://github.com/rladies/rladies.github.io>.

The site is a [Hugo](https://gohugo.io/) site built on a custom theme, `hugo-rladiesplus`, with [Tailwind CSS v4](https://tailwindcss.com), [Alpine.js](https://alpinejs.dev), and a small set of bundled vendor libraries for the chapter map, the events calendar, the directory filters, and the package hex-wall.
Most of what makes the site distinctive lives in the theme, not in the site itself.

If you are a contributor wanting to write a blog post, add a chapter, fix a typo, retire an organiser, or translate a page — the pages directly under `/website/` are for you.
If you are joining the website team and need to understand how the site itself is put together — how it builds, what feeds the directory, where the layouts live, how the GitHub Actions decide whether to deploy — read through the [Website Admin Guide](/website/admin_guide/).

The single most important thing to understand on a first read is that the [RLadies+ directory](https://github.com/rladies/directory) is the spine of the site.
Members, blog post bylines, the global team page, the package wall — they are all stitched together by `directory_id`.
Start with [The directory: where members live and how the site uses them](/website/admin_guide/directory-pipeline/) if you want to know why a small change in one repo can ripple across half the site.
