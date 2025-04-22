We hope to be able to translate the site into different key languages. 
There are several steps needed to add a global support for a new language to the site (in contrast to translating a single page).
First, the HTML ISO [letter language code](https://www.w3schools.com/tags/ref_language_codes.asp) for the new language should be established, and used consistently in the translation file contents and names. 

For all copied files to be edited to new language, the `.en` in the file name should be replaced with the HTML ISO language code for the new language.

- Add new language to `config/_default/languages.toml`
- Copy and edit `i18n/en.toml` to the new language
- Add a string in the `data/read_in_lang.yaml` which displays "Read in" string in the language.
- Copy and edit `data/month/en.toml` to the new language

Likely the best way to translate is to have the original language file (English) open at the same time with the file you are translating in to. This way it might be easier to catch what the original phrase was (if a translation is already attempted).  

If you are translating directly on GitHub, have a look at the instructions for [adding new mentoring entries](https://github.com/rladies/website/wiki/Adding-entries-to-the-mentoring-program-page) to the website, which also includes sections on how to commit and PR directly on GitHub. Tag @rladies/website for review and she'll get to it asap.

If you are comfortable working with git locally, clone the repo, make a branch and start translating. Once you are happy, push your branch to Github and do a PR. Tag @rladies/website for review and she'll get to it asap.
