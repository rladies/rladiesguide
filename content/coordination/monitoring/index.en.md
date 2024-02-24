---
title: "Monitoring chapter activity"
menuTitle: "Chapter monitoring"
weight: 45
---

### Goal

The goal of tracking chapter activity is to identify inactive chapters
and help them to get started. If nothing can be done for a particular
chapter, the chapter should be retired within 9months/1 year of
inactivity.

### Background

Chapters can be divided into 3 categories, according to the frequency of
their events:

-   Unbegun (created in the past 6 months, no events yet)

-   Active (have run an event in the past 6 months or have an upcoming
    event in the near future)

-   Inactive (no events in the past 6 months, no upcoming events)

### What to do

1.  Collate the list of inactive chapters and update the
    [InactiveChapters](https://docs.google.com/spreadsheets/d/1eLbqzqmBcrOIrhoWadl9uiKdcSJ1W-vWPMaXbMV8TuQ/)[^spreadsheet1]
    spreadsheet. 

2.  Contact INACTIVE chapters to check whether they need help. Use
    \<chapter\>\@rladies.org. Please, always send emails from your
    rladies email address. If you wish, you can use the email
    [template A](#email-template-a).

    c.  If organisers want to join the [mentoring program](/coordination/mentoring/) point them to
        the form to join the programme
        (https://rladies.org/form/mentoring-signup/) and inform the Mentoring Team.

    d.  If you find it difficult to deal with a particular request for
        help, the case can be discussed on the Global Team channel. 

3.  If no answer is received within 2 weeks contact organisers on slack
    (you can use [template A](#email-template-a)).\
    List of chapters and organisers:
    [https://github.com/rladies/starter-kit/blob/master/Current-Chapters.csv](https://github.com/rladies/starter-kit/blob/master/Current-Chapters.csv)

4.  If no answer is received on slack within 2 weeks, send another email
    (you can use [template B](#email-template-b)).

5.  Before the end of every month, please send an email update to
    `chapters@rladies.org`.
    Things to include in the email: list of chapters contacted, a
    summary of responses received and the list of chapters that need
    to be retired within the following month. **Date of retirement is
    expected to be within 9months/1 year from the date of creation or
    from the last event**.

6.  Retire chapters:

    e.  Delete page on meetup.com (this is done by the Meetup Team)

    f.  Send a Pull Request to change the
        status of the chapter to retire in the relevant file
        [on the chapters' data folder](https://github.com/rladies/rladies.github.io/tree/main/data/chapters)

    g.  Coordinate with the volunteer(s) responsible for the email accounts to: i.  delete rladies emails used by the chapter (organisers and
            chapter); ii. suspend social media accounts

    h.  Coordinate with the volunteer(s) responsible for the GitHub organization to delete team
        on GitHub

### Email template A

> First notification (via email and slack)

```markdown
SUBJECT: Your chapter has no recent activity, do you need help?

BODY OF THE MESSAGE:

Dear organisers,

Hope you are well! Just wanted to touch base and see how your chapter
is doing.

We noticed that your R-Ladies chapter was created more than 6 months
ago, there have not been recent meetups and there are no upcoming events
scheduled. Is there anything we could do to help you?

Here are some of the things we can support you with:

1.  If you are new to organising events, we could team you up with an
    experienced organiser from another chapter. If this is of interest
    to you, please join the mentoring programme by filling this form:
    [https://goo.gl/forms/MtVTIwBwUFYYW50B3](https://goo.gl/forms/MtVTIwBwUFYYW50B3).

2.  If you need quick advice, feel free to ask on the organisers'
    slack.

3.  If you no longer have time to dedicate to R-Ladies, let us know and
    we will try and find someone to help or take over from you.

Please get in touch as soon as possible and let's discuss options,
there are many!

If we do not hear from you, we will assume nobody is currently leading
the chapter and it will be scheduled to be [retired](https://guide.rladies.org/organization/intro/retiring/).

Please let us know what you plan to do, we are here to help.

Best wishes,
```

### Email template B

> Second notification via email (and possibly slack)

```markdown

SUBJECT: IMPORTANT, your chapter is scheduled to be retired!

BODY OF THE MESSAGE:

Dear organisers,

Hope you are well!

We noticed that your R-Ladies chapter has been inactive for a long
time.

We contacted you on \<chapter\_name\>\@rladies.org and on slack in the
last few weeks to see whether we could do anything to help. As we
received no answer, we assume you are no longer involved in running the
chapter and the meetup subscription can be terminated.

Unfortunately, the R-Ladies organisation has limited funding and we can
only support chapters that can run at least one remote/in-person meetup
event every 6 months (see our [rules](https://guide.rladies.org/coordination/monitoring/)).

This is to let you know that your chapter will be retired on \<INSERT
DATE HERE\>. Could you please send an email to chapters@rladies.org
with login credentials to the chapter's email/slack/twitter/facebook
etc. The leadership team will retire the chapter and close/suspend any
other related accounts.

We hope you will consider becoming a member of the R-Ladies Remote
chapter (https://www.meetup.com/rladies-remote/) and the R-Ladies
Community slack
([https://rladies.org/community-slack-form](https://rladies.org/community-slack-form)),
if you are not already.

Best wishes,
```

### Email template C

> If an organiser wants retire a chapter voluntarily

```markdown
SUBJECT: Your chapter is scheduled to be retired

BODY OF THE MESSAGE:

Dear organisers,

Hope you are well!

Many thanks for informing us that you can no longer lead your chapter.

This is to let you know that your chapter will be retired on \<INSERT
DATE HERE\>. Could you please send an email to chapters@rladies.org
with login credentials to the chapter's email/slack/twitter/facebook
etc. The leadership team will close/suspend these accounts.

We hope you will consider becoming a member of the R-Ladies Remote
chapter (https://www.meetup.com/rladies-remote/) and the R-Ladies
Community slack
([https://rladies.org/community-slack-form](https://rladies.org/community-slack-form)),
if you are not already.

Best wishes,
```

###  Email template D

> if a former organiser wishes to re-activate a retired chapter

(Contact Patricia once this email is sent and the corresponding chapter
replies confirming it's reactivation)

```markdown
SUBJECT: Guidelines to re-activate a chapter

BODY OF THE MESSAGE:

Dear organisers,

Hope you are well!

As you know, your chapter was retired for inactivity. When this happens
we consider the chapter inactive/retired, see status of your chapter
here:
https://github.com/rladies/starter-kit/blob/master/Current-Chapters.csv.

If you wish to re-activate your chapter, there are some steps to
follow:

1.  Please make sure only organisers that plan to contribute to this
    chapter in the future are listed in this file:
    [https://github.com/rladies/starter-kit/blob/master/Current-Chapters.csv](https://github.com/rladies/starter-kit/blob/master/Current-Chapters.csv).

2.  Chapter organisers must have access to the R-Ladies email account.
    It is fundamentally important to check this account regularly, as
    it is the main channel of communication between the organisation
    and the individual chapters. This account can be set up to forward
    to your personal email address. See https://guide.rladies.org//organization/tech/accounts/#e-mail

3.  All the accounts related to this chapter (e.g. social media) should
    be set up using the R-Ladies email account. Make sure you have
    access to all the chapter related accounts.

4.  As you certainly know, there is currently an expectation that a
    chapter runs at least 1 event every 6 months to remain active.
    Please familiarise yourself with the R-Ladies guidelines,
    especially if you are a new organiser.

5.  Given the current pandemic, you might want to consider waiting for
    the world to go back to normal or running a remote event. We
    invite you to take some time to think how your chapter is going to
    work in the future. When you are ready to run your next event,
    please follow the steps below:

    a.  Book the [date and time of your next event](/organization/events/online/)

    b.  Send an email to
        chapters@rladies.org
        explaining your plan to reactivate the chapter and keep it
        active in the near future. Include date/time and format
        (in-person Vs remote event) of your next event. If you decide
        to run a remote event, you can use our Zoom pro account.
    c.  There is no need to use the platform meetup.com, please let us
        know what platforms you plan to use to advertise and
        communicate with the members of your chapter in the future.

Should you have any further questions, please do not hesitate to
contact us.

Best wishes,
```

[^spreadsheet1]: You'll need to request access.
[^spreadsheet2]: You'll need to request access.