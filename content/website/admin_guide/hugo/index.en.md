---
title: Hugo setup
weight: 1
menuTitle: Hugo setup
---

## Hugo setup

This section contains information about the internal workings of the website.
It should provide an idea of how it works, and how you may contribute to it.

This hugo site is set-up as made possible within the [hugo](https://gohugo.io/) framework.
The following information should give you a general idea of how the website is configured and set-up.

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

Adding a new page is done by creating a new folder, and adding an `index.md` file to it.
The `index.md` file should contain the front matter, which is the metadata for the page, and the content of the page.
The front-matter (yaml) should contain the title, menu title, weight, and other metadata for the page.
Depending on the page, it may also contain other metadata, such as the date, author, and other information.

The content files are written in markdown, and can contain any markdown syntax.
The content files can also contain Hugo shortcodes, which are used to add functionality to the content.


### Shortcodes

Hugo shortcodes are used to add functionality to the content files.
Shortcodes are a way to add custom functionality to the content files, without having to write custom HTML or JavaScript.
Shortcodes are defined in the `layouts/shortcodes` folder of the theme, and can be used in the content files by using the shortcode syntax.

Currently, in addition to the [standard Hugo shortcodes](https://gohugo.io/content-management/shortcodes/), the site uses the following custom shortcodes:

- `button`: Used to create R-Ladies branded buttons in the content files.


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

### Menu

The menu is configured in the `config` folder.
To add a new menu item, you can add it to the `menu` section of the config file.
However, you will still not see the new menu item in on the website until you also add the menu item to the correct language `i18n` file.
The `i18n` files have a specific section for the menu items, which is used to translate the menu items into the different languages.
Once you've added the menu item to the `i18n` file, it will be displayed in the menu on the website.

### i18n

The i18n folder contains the translation files for the different languages.
These files are used to translate various sections of the the site into different languages.
To learn more about how to add a new language, see the [Adding a new Site Language](/website/multi-lingual/new) page.


### Theme

The theme for the website is an adaptation of the [hugo-initio](https://miguelsimoni.github.io/hugo-initio-site/) theme,specifically for this website.

The themes custom adaptations include:

- Maps in home and event page made with [amcharts](https://www.amcharts.com/docs/v4/tutorials/)
- Custom directory grid
- Custom global team grid
- Events calendar with [Toast UI calendar](https://ui.toast.com/tui-calendar)
- Multilingual mode
    - See [Adding a new Site Language](/website/multi-lingual/new) for how to add a new language to the site.
