---
title: "Frequently asked questions"
menuTitle: "FAQ"
weight: 99
---

## Why are you not using an R-package for the website?

There are several reasons we decided to create the website purely using Hugo, rather than going through an R package.

**1. The site does not have a lot of new content to be added**

The website does not need to render content from `Rmd` files.
The majority of updated content to this site is to the site's data (directory, chapters, and events), and standard text-information.
Since the majority of pages on the site does not need `.Rmd`, we have opted to use standard `md` for the site content.
In terms of the blog, this means knitting `Rmd` to `md` for the posts to be visible on the site.
The `md` files _and_ `Rmd`s for the blogposts should _both_ be committed to history. This way old blogposts need not be re-knit.

**2. Making the site more stable & fast to render**

Doing everything in hugo directly gives us one less software that might introduce breaking changes to the site.
Hugo also builds the site very fast on its own, meaning new changes are quickly implemented on the rendered site.

**3. Making use of more advanced Hugo options**

As the R-Ladies website is meant for a global audience, it means we also need to build a site where we can incrementally add more language translations over time.
Utilizing Hugo's systems for translated content, when wanting to support high amounts of it, is not 100% supported in, for instance, blogdown.
In particular, using the `config`-directory option, rather than a single `config`-file is convenient as we can more tidily organise translated config options.
