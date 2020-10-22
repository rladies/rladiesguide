---
title: "Editing and publishing the Code of Conduct"
menuTitle: "Code of Conduct changes"
weight: 11
---

The Code of Conduct would only be edited with the agreement of the global leadership.

Now, technically, its source including translations live at https://github.com/rladies/.github/blob/master/CODE_OF_CONDUCT.md

If you use the Code of Conduct somewhere that's supposed to be long-lived, be careful to have a process in place to ensure that the Code of Conduct rendered on your thing keeps in sync with the code of conduct source.

For an event that happens only once, you could copy-paste from the code of conduct source.

## How to use the .github COC in a Hugo website 

This is a suggested workflow.
It is very important to have a process in place for keeping the code of conduct content in your website in sync with the code of conduct source.

* Read the code of conduct from GitHub in a layout e.g. https://github.com/rladies/rladiesguide/blob/75298828dd20a08d40176afcfbccc8ca12cfd344/layouts/coc/single.html#L4-L5
* Have the time of the latest build appear explicitly e.g. https://github.com/rladies/rladiesguide/blob/75298828dd20a08d40176afcfbccc8ca12cfd344/layouts/coc/single.html#L3
* Link to the code of conduct source. See [the code of conduct in this very guide](/about/coc/)
* Make sure there is a process rebuilding your website when the code of conduct changes. If your website is deployed with Netlify
    * On Netlify generate a build hook for the website. It is an URL.
    * If you have access to the rladies/.github repo add the URL in a repo secrets, and add a line to https://github.com/rladies/.github/blob/master/.github/workflows/main.yml that ensures a build of your website will be triggered every time the COC is updated. If you don't have access, ask someone with access.