---
title: Website maintenance
weight: 9
chapter: true
menuTitle: Website maintenance
---

This wiki contains information about the internal workings of the website.
It should provide an idea of how it works, and how you may contribute to it.

This hugo site is set-up as made possible within the [hugo](https://gohugo.io/) framework.
The following information should give you a general idea of how the website is configured and set-up.

## Hugo setup

### Configuration

The site is configured using a [config directory](https://gohugo.io/getting-started/configuration/#configuration-directory) rather than a single config-file.
This makes is easier to create large custom configurations but keep the structure clean.

In particular, the config directory makes it easier to keep the menu definitions for the different site languages distinct.

### Content

The content folder has all the content for the website.
Unlike Hugo content made from R-packages, this site does not use `Rmd` as content source.

The content also uses [page bundles](https://gohugo.io/content-management/page-bundles/) to make it easier to keep files organised together neatly.
A bundled page means that the page has all its content in a folder (where the folder name is the page name/slug), an `index.md` which is the main content file and all secondary files (images etc.) are nested within this page.
This makes it possible to use relative paths in the `index.md` to the secondary files, while also keeping a neat file structure.

**Note**
This repo comes with a project `.Rprofile` that has some settings for the website if you use blogdown to work with locally.
Do not alter the project `.Rprofile` but supplement it with your own profile if you like.
The settings in this file makes it possible to work with blogdown (when blogdown starts supporting config-folders).
It is set up so that creating new posts and pages use the correct settings for the website (like knitting to markdown, rather than html, bundling pages etc).

### Data

The site used hugo [data templates](https://gohugo.io/templates/data-templates/#readout) for several purposes.

- Event data
- Chapter information
- Month name translations

Hugo data template source data are found in the `data` folder, where we have chosen to use `.json` files for this purpose.

Of these, only the mentoring program has a file that is updated manually by R-Ladies global team members working with the mentoring program.

The month names are also created by us, but once created should not need changes as long as they are correct. The month name jsons are used when site language is switched, for displaying the correct month names in the chosen language.

### Static

In the static folder, all files that should be globally accessible to the content files can be placed.
Currently, this only contains images.
If a file is specifically used for a single content file, it should be bundled with the page, rather than placed in static.
If is a more general purpose file (like R-ladies logo etc) to be used in multiple files, it is best to place it in static and refer to it by its relative path to the base-url of the page.

```
/images/logo.png  # Looks for the image as placed in static/images/logo.png
images/logo.png   # Looks for the image as relative to the content index.md file
```

### Theme

The theme for the website is an adaptation of the [hugo-initio](https://miguelsimoni.github.io/hugo-initio-site/) theme ,specifically for this website.

The themes custom adaptations include:

- Maps in home and event page made with [amcharts](https://www.amcharts.com/docs/v4/tutorials/)
- Custom directory grid
- Custom global team grid
- Events calendar with [Toast UI calendar](https://ui.toast.com/tui-calendar)
- Multilingual mode
