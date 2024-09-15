---
title: "Onboarding new chapter organizers"
menuTitle: "Organizer onboarding"
weight: 1
---

## Goal

The goal of this activity is to start a conversation with prospective
organisers, answer any questions and send information on how to get a
new chapter started. This activity can be carried out by a team of 2
people, to split the workload and guarantee requests are addressed
timely.

## What to do

-   Monitor the email: `chapters@rladies.org`. As part of the Onboarding
    Team, the messages to this account are forwarded to the email
    address you provided during your own onboarding process.

-   On the rladies.org website, we ask any prospective organisers to get
    in touch via email. Especially during conferences, we may receive
    a high volume of requests for information. If possible, this email
    address should be monitored daily.

-   If there is a request for activation of a new chapter, we need the
    following information:

    -   City/Region/Country where the meetup will be located

    -   Name(s) of organiser(s) and their email address (to be added to
        the the R-Ladies Community slack).

-   Open a new "new-chapter-setup" issue that will help you track the execution of the following instructions:     

    -   Make a search on GitHub
    ([https://github.com/rladies/rladies.github.io/tree/main/data/chapters](https://github.com/rladies/rladies.github.io/tree/main/data/chapters))
    to make sure the city does not have a chapter yet. You can use CTRL+F to search using the city's name.

    -   If there is a chapter already, check the name of the organisers match.
    
    -   Ask the person to confirm they identify as a woman or gender minority and are interested in the R programming language (just an ok, we are not collecting information). For alignment with the R-Ladies mission, can you just write an ok as an answer to "Do you identify as a woman or gender minority, and are you interested in the R programming language?" (really just an ok, we're not collecting personal data!)

    -   If there is no chapter, check if there is a chapter nearby. If so, inform the sender about it and put them in contact with the chapters organizers by adding the email address of the chapter as CC.

    -   If there is no local chapter yet:

        -   We make sure it is a city using Google Maps

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

        - Add prospective chapter to the current chapters on the website.

-   **Other types of requests:**

    -   People may ask to be added to the organiser slack, please check
        if their name is in the meetup list of co-organisers. If not,
        ask why.

    - Onboard new organizers:
      - Ensure you have received the email addresses of the new organizers
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

    -   General information -\point them to the Community Slack and
        meetup dashboard (see [template C](#appendix-c))

## Appendix A

> Onboarding email template

```markdown
Hi \<\<first name\>\>!

It's great to hear that you are interested in starting R-Ladies
\<\<city chapter\>\>!

We have some information on starting a local chapter here:
https://guide.rladies.org/

We also have a Slack group which I'll send you an invitation to in a
minute. The Slack group is mainly how we communicate and a great place
to discuss meetup organization and other matters (R-related and
other). Once you join, please say hello and introduce yourself in the
*\#welcome* channel. When you are ready to launch your chapter and start
advertising on social media, please go to #new_chapters and ask:

-   The meetup team to set up a new meetup page for you,

-   The email team to create a chapter email address, you will
    need this to set up social media accounts.

In the Slack, together with all R-Ladies chapters organizers worldwide,
you will find the
R-Ladies Global leadership team. Feel free to reach out to them through
the channel #ask_the_leadership if you have questions that are not
addressed in the Guidelines or that other organisers
cannot answer. More info about the R-Ladies Global team is can be found on the R-Ladies website:
https://rladies.org/about-us/team/.

There is no specific time frame for launching the chapter. Note that if
other persons are interested in organizing this chapter, they will be
onboarded to join you in the organization. You will be notified of this
and we will put you in contact with any other person interested. The
chapters organizer team will remain open until the first meetup takes
place. After the first meetup, the local organizer team will be in
charge of any further decisions about local leadership.

Once your chapter launches on the meetup.com platform, please hold your
first event within the next 6 months. After that, please keep your
chapter active with at least one event every 6 months. For comparison,
many chapters do an event every 2-3 months and some chapters do monthly
events. If you find yourself struggling with this frequency, please be
in touch and we'll figure something out.

Welcome!

\<YOUR NAME HERE\>

R-Ladies Global Team

\<\*\add if they mention other organizers not cced in the original
message: "Please send us the names and emails of the other organizers so
we can add them to our records."
```

## Appendix B

> confirm list of organisers - email template

```markdown
Hi \<\<first name\>\>,

Thanks for sending through the updated list of organisers. I sent the
invites to the new organisers to join the R-Ladies Community Slack.

We keep information about chapters and organisers on this list on
GitHub:
https://github.com/rladies/rladies.github.io/tree/main/data/chapters.

-   Could you please send the email addresses, names, and surnames of the new organizers so we can update the chapter list?

-   Could you also please confirm whether I should remove the following
    from the list of organisers?

    -   \<\<NAME1\>\>

    -   \<\<NAME2\>\>

We assume former organisers provide all the necessary information to new
organisers before passing the baton :)

Kind regards,

\<YOUR NAME HERE\>
```

## Appendix C

> general info - email template

```markdown
Hi \<\<first name\>\>,

Thanks for reaching out.

My recommendation would be to join the R-Ladies community slack channel
(https://rladies.org/form/community-slack/) and start a
conversation with fellow R-Ladies.

Also, please check out this dashboard to find out whether there is a
chapter near you https://www.meetup.com/pro/rladies.

Hope this helps.

Kind regards,

\<YOUR NAME HERE\>
```
