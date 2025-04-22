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
