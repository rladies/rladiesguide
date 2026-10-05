---
title: "Onboarding new chapter organizers"
linkTitle: "Organizer onboarding"
weight: 1
aliases:
  - /coordination/onboarding/
---

## Goal

The goal of this activity is to start a conversation with prospective
organisers, answer any questions and send information on how to get a
new chapter started. This activity can be carried out by a team of 2
people, to split the workload and guarantee requests are addressed
timely.

## What to do

- Monitor the email: `chapters@rladies.org`. As part of the Onboarding
  Team, the messages to this account are forwarded to the email
  address you provided during your own onboarding process.

- On the rladies.org website, we ask any prospective organisers to get
  in touch via email. Especially during conferences, we may receive
  a high volume of requests for information. If possible, this email
  address should be monitored daily.

- If there is a request for activation of a new chapter, we need the
  following information:

  - City/Region/Country where the meetup will be located

  - Name(s) of organiser(s) and their email address (to be added to
    the the R-Ladies Community slack).

- Open a new "new-chapter-setup" issue that will help you track the execution of the following instructions:

  - Make a search on GitHub
    ([https://github.com/rladies/rladies.github.io/tree/main/data/chapters](https://github.com/rladies/rladies.github.io/tree/main/data/chapters))
    to make sure the city does not have a chapter yet. You can use CTRL+F to search using the city's name.

  - If there is a chapter already, check the name of the organisers match.

  - Ask the person to confirm they identify as a woman or gender minority and are interested in the R programming language (just an ok, we are not collecting information). For alignment with the R-Ladies mission, can you just write an ok as an answer to "Do you identify as a woman or gender minority, and are you interested in the R programming language?" (really just an ok, we're not collecting personal data!)

  - If there is no chapter, check if there is a chapter nearby. If so, inform the sender about it and put them in contact with the chapters organizers by adding the email address of the chapter as CC.

  - If there is no local chapter yet:

        -   We make sure it is a city using Google Maps, and that the name
            follows [the naming policy](#naming-a-chapter)

        -   We add the chapter to the chapter data (https://github.com/rladies/rladies.github.io/tree/main/data/chapters), only the info below
            (no email address):

            -   City

            -   Region (if relevant)

            -   Country

            -   Organisers

            -   Status = Prospective

        -   Invite organisers to the R_Ladies Community Slack.

        -   Invite organisers to the R-Ladies organisers Slack once they fill in the R-Ladies form.

        -   Send the prospective organiser an email with all the most important

    information and links (see [Appendix A](#appendix-a))

        - Post a message in the issue to request @rladies/email and @rladies/meetup-pro to create the chapter infrastructure.

        - Create a PR to add prospective chapter / chapter organizers to the current chapters on the website.

        - Request a review of the PR from `rladies/leadership`. Once the PR is approved and the GitHub Actions pass, the person who submitted the PR is responsible for merging it.

- **Other types of requests:**

  - People may ask to be added to the organiser slack, please check
    if their name is in the meetup list of co-organisers. If not,
    ask why.

  - Onboard new organizers:

    - Ensure you have received the email addresses of the new organizers.
    - Open a new "existing-chapter-update" issue to track the exectution of the following instructions:
      - Send new organizers the R-Ladies Organizers form.
      - Invite the new organizers to the R-Ladies Community Slack.
      - Invite the new organizers to join the organizers' Slack workspace after they fill in the R-Ladies Organizers form.
      - Post a message on the open issue to request the Meetup and Email teams to update the chapter infrastructure.
      - Request the Website team to add the new organizers to the website.
    - The [template B](#appendix-b) can be used as a response to this type of request.

  - Retire organizers:

    - Request the Meetup team to change the status of co-organizers stepping down to members on Meetup.
    - Update the chapter's information [on the website](https://github.com/rladies/rladies.github.io/tree/main/data/chapters) by moving the current organizers to the "former organizers" field.

  - General information -\point them to the Community Slack and
    meetup dashboard (see [template C](#appendix-c))

## Naming a chapter

A chapter is named for the **city** it serves.
Not the country, not the state or province, not a broad region.

This matters more than it looks.
A chapter called after its country reads as though it speaks for every chapter in that country,
and it blocks the name that a future chapter in the capital would want.
We have four chapters in Saudi Arabia and five in Chile;
a group called "R-Ladies Saudi Arabia" leaves no room for the other three.

Check the city exists on a map before agreeing the name,
and check no nearby chapter already covers it.

### The name a chapter actually shows

The chapter page on rladies.org takes its title from the **Meetup group name**,
not from the chapter data.
So the name is settled when the Meetup group is created,
and changing it later means asking the chapter to rename their group —
the Global Team cannot do it for them.

Get it right at creation.

### Metropolitan areas are fine

A single well-known metropolitan area is an accepted exception,
even where it spans more than one city:

- `R-Ladies Twin Cities` — Minneapolis and Saint Paul
- `RLadies+ RTP` — Research Triangle Park, spanning Raleigh, Durham and Chapel Hill

The test is not "is this exactly one city".
It is **"does this name cover territory another chapter already has, or would want?"**
A metro area that functions as one place is fine.
A country or a vague region is not.

### Disambiguation is fine

Where two chapters share a city name, adding the state or country is right, not wrong:

- `R-Ladies London, Ontario` — distinct from London, UK
- `R-Ladies Athens Greece` — distinct from Athens, Georgia, which is also a chapter

### Branding

Chapters name themselves.
We do not convert a chapter's own name to "RLadies+" house style,
and chapters are free to adopt it in their own time.
The policy above is about _what place_ a chapter is named for, not how it spells RLadies+.

## Appendix A

> Onboarding email template

{{< template-file "chapter-onboarding-welcome" >}}

## Appendix B

> confirm list of organisers - email template

{{< template-file "chapter-organisers-confirm" >}}

## Appendix C

> general info - email template

{{< template-file "chapter-general-info" >}}
