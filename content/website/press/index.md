---
title: "Adding a mention of R-Ladies in the Press"
menuTitle: "Add press entry"
weight: 3
---

Adding new press entries means adding a new markdown entry in `content/activities/press`.
This is a folder where there are subfolders for each press entry.
Each folder is a date of a press entry (if there are multiple entries for the same date, please append the folder name with the media outlet name).
For instance: `2021-10-30-times` `2021-10-30-buzzfeed` `2021-12-23`

Inside this folder, there should be a single `index.md` with the following information in the yaml

```
---
title: "El aporte silencioso a la agroinformática"
date: 2017-06-26
source: "http://ria.inta.gob.ar/contenido/el-aporte-silencioso-la-agroinformatica"
---
```

- _title_: Article title in original language
- _date_: date the article was published (YYY-MM-DD)
- _source_: full URL to the article

Additionally, if there is a pdf available of the article, this can be placed in the same folder as the `index.md` file. It could in theory have the same name, but please give it a sensible name so that when someone accesses it the file name is meaningful.
For instance `2021-10-31-times-the-article-title.pdf`
As long as it is a pdf, the link to the pdf will appear on the press page.

Please follow the instructions in how to [contribute a blogpost](/comm/website/blog), to follow the git and github steps needed to contribute any file you may create.

Thank you for helping us!
