---
title: "Adding a chapter"
menuTitle: "Add chapter"
weight: 3
---

## Create a new file

Create a new file in the [data/chapters/](https://github.com/rladies/website/blob/main/data/chapters) folder by [using this link](https://github.com/rladies/website/new/main/?filename=data/chapters/country-state-city.json&value=%5B%0A%20%20%7B%0A%20%20%20%20%22urlname%22%3A%20%22rladies-%22%2C%20%20%20%20%20%20%20%20//meetup%20link%0A%20%20%20%20%22status%22%3A%20%22%22%2C%20%20%20%20%20//%20prospective%2C%20active%20or%20inactive%0A%20%20%20%20%22country%22%3A%20%22%22%2C%20%20%20%20%20%20%20%20%20%20//%20country%2C%20capitalised%0A%20%20%20%20%22state.region%22%3A%20%22%22%2C%20%20%20%20//%20state%20or%20reigion%2C%20capitalised%2C%20optional%0A%20%20%20%20%22city%22%3A%20%22%22%2C%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20//%20city%2C%20capitalised%0A%20%20%20%20%22social_media%22%3A%20%7B%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20//%20social%20media%20links%0A%20%20%20%20%20%20%22meetup%22%3A%20%22rladies-%22%2C%20%0A%20%20%20%20%20%20%22twitter%22%3A%20%22%22%2C%0A%20%20%20%20%20%20%22email%22%3A%20%22%22%0A%20%20%20%20%7D%2C%0A%20%20%20%20%22organizers%22%3A%20%7B%0A%20%20%20%20%20%20%22current%22%3A%20%5B%5D%2C%20//%20comma%20separated%20and%20each%20item%20quoted%0A%20%20%20%20%20%20%22former%22%3A%20%5B%5D%0A%20%20%20%20%7D%0A%20%20%7D%0A%5D). This link will fork the repository to your user account, and initiate a new file with some template content in it.

## File name

The name of the file should be the chapters identifier, with country (state/region if applicable) and city. This way we can ensure each file has a unique name and that duplication does not happen.
Any spaces should be substituted with a dash (`-`).

## File content

Fill in the appropriate information in the json.
Make sure to remove everything after the double slashes (`//`) in the final file, as these are just comments to help you fill out the information properly.
If the chapter does not have a state/region to apply, the entire line may be removed (rather than left empty).

example file:

```json
[
  {
    "urlname": "rladies-Algiers",
    "status": "prospective",
    "country": "Algeria",
    "city": "Algiers",
    "social_media": {
      "meetup": "rladies-Algiers",
      "email": "algiers@rladies.org"
    },
    "organizers": {
      "current": ["Kamila Benadrouche", "Lounas Fairouz", "Zibani Fazia"],
      "former": []
    }
  }
]
```

### Social media options

There are a set of currently available social media to be added.
If you think a new social media outlet should be added to the list, contact a website maintainer.

```
"twitter": "username"
"github": "username"
"instagram": "username"
"youtube": "username/end-url"
"tiktok": "username"
"periscope": "username"
"researchgate": "username"
"website": "url"
"linkedin": "username"
"facebook": "username"
"orcid": "member number"
"meetup": "end-url"
"mastodon": "@username@server"
```

## Commit and PR the file

At the bottom of the page on GitHub, add a commit message in the box.

You will immediately be sent to the 'Pull requests' page, to create a PR to the main branch.
Click the `Create pull request` button.
Once this is done, a new page will open and some automated checks of your submitted entries start.
In the comment section, make sure to @drmowinckels so she can take a look.

If anything needs fixing you will be notified and given instructions on how to do that.

Once all checks pass and the entries have been reviewed, they will be merged to the main branch.
