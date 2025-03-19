---
title: "Abstract Review System"
menuTitle: "Abstract Review"
weight: 105
---

R-Ladies Global has a system for review and feedback of conference abstracts and funding opportunities for both R-related and domain-specific conferences. The aim is to encourage women and other gender minorities to submit proposals and improve chances of having work or applications selected, and international reviewers provide constructive feedback on areas including topics and pitch, phrasing, and subject matter.

## User/volunteer facing docs

* [Submit a proposal/abstract for review](https://rladies.org/form/abstract-request/) ( Please request feedback no later than three weeks before your conference deadline. )

* [Become a reviewer](https://rladies.org/form/abtract-volunteer/)

## Behind-the-scenes

### Promotion

The system can be promoted on R-Ladies social media once in a while, 
in particular before big conference deadlines, 

* By R-Ladies social media

* In the [organizers slack](/organization/tech/accounts/#slack)

* In the [community slack](/comm/slack/)

* And anywhere where it might be useful.

### Coordination

The requests and volunteers are tracked in AirTable: https://airtable.com/appJadVolZxoDGSIK?

Volunteers should decide on the best way for them to manage reviews, for example, alternating between team members on a monthly basis.

## Handling an incoming request

Incoming requests trigger an automated message to be posted in the #team-abstract_review channel in R-Ladies Slack.

The review handler should:

1. Check the request and the submission deadline, preferred reviewer language and gender, comments made by requestor, and that any attached documents are accessible
2. Select a relevant reviewer from the list of volunteers, incrementing the value in the "reviews requested" column by 1.  If possible, try to use volunteers who haven't done too many previous reviews.
3. Email 3 potential reviewers using the template below or similar
4. Regularly check in to make sure we're not awaiting progress - see if the reviewer has added comments to the doc, and if they don't respond to the initial email assign a new reviewer
5. Update the "Status" and "Reviewer 1", "Reviewer 2", and "Reviewer 3" fields in the "abstracts" table with the relevant info
6. Update the "Reviews requested" and "Reviews completed" fields in the "volunteers" table with the relevant info


## Template for getting in touch with reviewer

Hi <reviewer name>!

Thank you so much for offering to review abstracts through the R-Ladies review program! We received a request earlier and I wondered if you'd have time in the next week or so to take a look?  The conference deadline is <date>.

If you do, the link is <here - add link>, and I think you should be able to leave comments directly in the doc. If now isn't a good time, totally understand - just let us know so we can assign a new reviewer!

Thanks!

<your name>

