The website has something called a "redirect" type of page.
The redirect page enables us to redirect people from our website to somewhere else when we want to.
This is a very nice way to generate official, short and pretty urls from our website URL to somewhere else.
For instance, at Posit::conf 2023, we used the link `rladies.org/postit23` to redirect to a slido for attendants to ask us questions.

Currently, we use it mostly to redirect to R-Ladies forms we use in the global team.
Such forms are used to help the global team keep track of tasks, requests etc.
These forms occasionally change, and remembering where all occurrences of an old link exists is very difficult.
Transitioning into using the website redirects in stead, we can use the official redirect links from our website in all materials, and when the form urls change, we need only change the redirect url on the website.
If all other materials use the rladies.org/url , then a change on our own website is all that is needed to have all new visitors use the new form.

The redirect pages only contain yaml information:

```yaml
---
type: redirect
redirect: https://app.sli.do/event/eugGEUP3oRHabkNiTBEhm6
title: "R-Ladies Posit::Conf 2023 meetup"
---
```

These three bits of information are the only things needed for the page and redirect to work.
It is also not necessary for a redirect to exists forever.
The exampled posit conf redirect was no longer needed after the conference, for instance.

All the redirects to forms are located under `content/form/` which helps us keep track of all the forms we have, and lists them neatly under rladies.org/form.
The location of this file is something to discuss with the website team, and depends on the need.
If you do not know where it belongs, initiate an issue and describe the redirect wanted, and the team will decide.
After this, you can try creating a redirect in that location, and do a PR into the repo.

Some redirects may also use Hugo's `alias` function, to also use previous locations of a redirect file on the website.
The R-Ladies directory form is such an example:

```yaml
---
type: redirect
redirect: https://airtable.com/appzYxePUruG9Nwyg/pagF4TCWTbkjfuyLn/form
title: "R-Ladies Directory form"
alias:
  - /directory/update/
  - /directory-update/
  - /directory-update
---
```

This form was the first ever pretty url we used, and as such, we did not have any clear rules on where the placement of this file should be.
Its had several homes, and as such, we have created three aliases for it because of its previous locations.
What this means is that any of the locations of `rladies.org/directory/update`, `rladies.org/directory-update` or `rladies.org/form/directory-update` will redirect to the correct redirect url specified in this file.

## How to create a new redirect

To create a redirect (pretty URL), for instance from `rladies.org/form/awesome-thing` to `horribly-long-random-airtable-link-that-we-might-change-anyway`,

- create a Git branch.
- use the [redirect archetype](https://github.com/rladies/rladies.github.io/blob/main/themes/hugo-rladies/archetypes/redirect.md).
- fill it.
- save it under the correct name and location, for instance `content/form/awesome-thing.md` for `rladies.org/form/awesome-thing`.
- optional: test it locally.
- open a PR, test the redirect in the preview: `rladies.org/form/awesome-thing` should redirect to `horribly-long-random-airtable-link-that-we-might-change-anyway`.

When sharing a form link, please use `rladies.org/form/awesome-thing`, **not** `horribly-long-random-airtable-link-that-we-might-change-anyway`.
