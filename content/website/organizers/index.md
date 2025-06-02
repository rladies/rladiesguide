---
title: "Updating chapter organisers"
menuTitle: "Update chapter organisers"
weight: 4
---

There are 2 administrative tasks which require completing when retiring former organisers:

1. changing their role on meetup.com

2. updating their status in our JSON data

The instructions below outline each of these.

## Task 1. Changing organizer role on meetup.com

Note: for some reason I had issues with getting this working in Firefox, but it seemed to work fine in Chrome 🤷

1. Login to meetup.com with the RLadies Global account. Select the “Pro Dashboard” from the top right of the page. Search for the meetup group containing the former organiser and go to its page.

2. In the “Organizers” panel, click on “R-Ladies Global and N others” to be taken to the Organizers admin page.

3. Find the member you wish to retire and click on the “...” on the right next to their name. Select “Change member role”.

4. On the menu that appears, select “member” and then click “update role”

## Task 2. Updating Organizer Status in the Directory

1. Head to https://github.com/rladies/rladies.github.io/tree/main/data/chapters and find the relevant chapter. Click to open the JSON file associated with that chapter.

2. Click on the “edit” button in the top right of the code view. It looks like a little pencil.

3. You should now be able to edit the JSON file. Move the retired organizer from the “current” to the “former” field and select “Commit Changes” and then “Propose Changes” and then “Create Pull Request”.

4. Double-check the details and then submit the pull request! Some CI checks will run on the PR to make sure the JSON is correctly formatted, and the relevant people will be notified to review/merge it.

(You can achieve the above by manually making a fork of the repo and then editing the file locally and then making a pull request, although I found this method to be quicker and easier)
