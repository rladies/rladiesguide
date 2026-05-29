---
title: "Organisation, OUs, Groups, and policies"
linkTitle: "Organisation & policies"
weight: 40
---

The default Google Workspace tenant places every user account — chapter mailbox, personal alias, service account — in one bucket with one set of policies.
That works while the org is small, but every new chapter widens the policy compromise: enforce strict 2FA and chapter co-organisers can't share access; relax it and the leadership accounts inherit the looser stance.
The fix is structural.

This page documents the recommended target shape for the RLadies+ Workspace tenant — the Organisational Units, Groups, lifecycle conventions, and DNS-level security that the current flat layout doesn't yet have.
Almost nothing here is already implemented; treat the page as the plan, not the snapshot.
Each section names what's in place today versus what's a TODO for the maintainer.

## Why structure matters now

Today the tenant is essentially flat: one default Organisational Unit holds every mailbox, a handful of Groups exist but aren't enumerated, and chapter mailboxes sit under the same authentication policy as the role mailbox.
The friction this creates is mostly invisible until the day a policy needs to change.
At that point every change is global — there's no way to enforce hardware-key 2FA on the super-admin without simultaneously demanding it of a Buenos Aires co-organiser who shares the mailbox over Signal.

Splitting the tenant into Organisational Units gives the admin a place to apply different policies to different kinds of account.
Splitting routing into named Groups gives the admin a way to add and remove people from distribution lists without rewriting BCC fields.
Both are cheap to set up while the directory is small; both compound in value as the chapter list grows.

## Organisational Units

The recommended OU layout has five buckets, each justified by a policy difference the current flat tenant can't express.

TODO: maintainer to create these OUs in admin.google.com and apply the policies described.
None of the OUs below exist in the tenant yet beyond the default root.

| OU                  | Members                                                                   | Why this OU                                                                                                                                                                                                                                                                                              |
| ------------------- | ------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `/Service accounts` | `accounts@`, future role mailboxes                                        | Strictest 2FA (hardware key or passkey), shortest session length, full audit logging. These accounts hold elevated privilege and have no day-to-day human user who'd object to the friction.                                                                                                             |
| `/Global Team`      | Personal `name@rladies.org` for leadership and Global Team members        | Enforced 2FA, app-access controls that match a real person's working pattern. These users sign in regularly and can be expected to maintain their own credentials.                                                                                                                                       |
| `/Chapters/Active`  | All current chapter mailboxes                                             | 2FA recommended but not forced. Chapter mailboxes are shared between co-organisers, and enforced 2FA on a shared inbox tends to push organisers towards storing the second factor somewhere it shouldn't live. Broader app access since organisers genuinely use Calendar, Meet, and occasionally Drive. |
| `/Chapters/Dormant` | Chapter mailboxes whose chapter has gone quiet but isn't formally retired | Login suspended; data retained. Acts as a holding pen between "active" and "deleted" so a chapter revival doesn't lose its history.                                                                                                                                                                      |
| `/Alumni`           | Departed Global Team personal aliases in their grace period               | Login disabled, forwarding only. See [Personal alias offboarding](#personal-alias-offboarding) below for the timeline.                                                                                                                                                                                   |

The literal click path for creating an OU:

1. Sign in to [admin.google.com](https://admin.google.com) as the role mailbox using the credentials from [1Password]({{% relref "/global-team/infrastructure/1password" %}}).
2. In the left sidebar, open **Directory → Organizational Units**.
3. Click **+ Create new organizational unit** at the top of the page.
4. Fill in the **Name** (e.g. `Service accounts`) and an optional **Description** explaining the policy intent.
5. For nested OUs like `/Chapters/Active`, first create `Chapters`, then create `Active` with `Chapters` selected as the parent.
6. Click **Create**.

To move a user into an OU after the OU exists:

1. Open **Directory → Users** and find the user.
2. Click their name to open their profile.
3. Click **More options** (the three-dot menu) and choose **Change organizational unit**.
4. Select the target OU and click **Continue**, then **Change**.

TODO: maintainer to define the policy bundle (2FA enforcement level, session length, app access) on each OU once created.
The OU is just the container; the policies are configured separately under **Security** and **Apps** in the admin console and need to be reviewed against the table above.

### Worked example: enforcing 2FA on `/Service accounts`

The canonical case is forcing 2-step verification on the service-account OU.
The same select-OU-then-set pattern applies to every other policy in the admin console — session length, app access, less-secure-app permissions — so this example doubles as the template for the rest.

1. Sign in to [admin.google.com](https://admin.google.com) as the role mailbox.
2. Open **Security → Authentication → 2-step verification**.
3. In the left-hand OU selector, click **Service accounts** so the panel scopes to that OU only — the heading should now read "Settings for Service accounts" rather than the root org.
4. Tick **Allow users to turn on 2-step verification** if it isn't already on.
5. Under **Enforcement**, choose **On** (or **On from a specific date** if a grace period is needed).
6. Set the **Enrollment period** to something short — two weeks is enough for a single role mailbox — so accounts that haven't enrolled get blocked rather than living indefinitely without 2FA.
7. Under **Methods**, restrict to **Security key** or **Any except verification codes via text, phone call** so the strictest factor is required for the strictest OU.
8. Click **Save**.

Every policy in the admin console can be scoped to an OU using this same pattern: open the policy, select the target OU from the left-hand selector, set the values, save.

## Groups strategy

Right now distribution happens through ad-hoc BCC lists and personal memory of who's on which team.
That's the failure mode where a new finance lead doesn't get looped in on a vendor email for three months because the previous lead's mental BCC list never made it into anyone else's head.

Formalising routing through Google Groups fixes the handover problem.
A Group has membership the admin can read off a page; a BCC list has membership in someone's drafts folder.

Recommended Groups, with the recommended access type in parentheses:

- `leadership@rladies.org` (Only members can post; members-only visibility) — routes to whoever holds Leadership roles this year. Used for external partners who need "someone in charge".
- `global-team@rladies.org` (Only members can post; members-only visibility) — routes to every active Global Team member's personal alias. Used for internal coordination that everyone needs to see.
- `directory@rladies.org` (Anyone on the web can post; anyone in the org can view members) — routes to the Directory team for chapter listing and onboarding questions. This one is public-posting on purpose because would-be chapter leads need to reach it from any address.
- `mentoring@rladies.org` (Anyone on the web can post; members-only visibility) — routes to the Mentoring team for programme enquiries.
- `social-media@rladies.org` (Only members can post; members-only visibility) — routes to the social media team for cross-platform requests.
- `finance@rladies.org` (Only members can post; members-only visibility) — routes to whoever currently handles invoicing and reimbursements.
- `chapters@rladies.org` (Only members can post; anyone in the org can view members) — routes to every active chapter mailbox. Used sparingly, for genuinely-org-wide announcements only.
- `chapters-new@rladies.org` (Only managers can post; members-only visibility) — routes to chapters in their first six months. Used for onboarding-specific announcements that established chapters can skip.

Access type lives on the group's Settings page at `groups.google.com/a/rladies.org/g/<group>/about` (open the group, click the gear icon, then **General**).
This setting matters disproportionately because the wrong choice on a public-posting group is exactly how spam arrives at hundreds of inboxes at once: a single open list that fan-outs to every chapter mailbox is the difference between zero spam and a coordinated wave.
Set it deliberately at create time and review it whenever a group's purpose changes.

Every group needs at least one Owner who is a human, in addition to the role account.
At create time, set the team lead as Owner and the role mailbox as a secondary Owner (or as the only Owner if no team lead exists yet).
The convention: every group has at least one human owner besides the role account, so a single departure doesn't leave the group unmanaged.

Optional regional sub-groups (`chapters-emea@`, `chapters-americas@`, `chapters-apac@`) become worth the maintenance overhead once an announcement is genuinely regional rather than global.
Leave them out until that need is concrete.

The literal click path for creating a Group:

1. Sign in to [admin.google.com](https://admin.google.com) as the role mailbox.
2. Open **Directory → Groups**.
3. Click **+ Create group**.
4. Fill in **Name** (human label), **Email** (the `@rladies.org` address), and **Description** (one line explaining what the group routes).
5. Set **Access type** — for most of the Groups above this is _Restricted_ (only members can post) or _Team_ (members plus other org users can post), depending on who legitimately needs to email the list.
6. Click **Create group**, then add members from the next screen.

TODO: maintainer to enumerate the Groups that already exist in the tenant and reconcile with the list above — some may already be in place, some may need creating, and any orphaned groups from previous setups should be cleaned up.

## Chapter mailbox lifecycle

Chapters go quiet.
Sometimes they come back, sometimes they don't, and the current process for deciding when to delete a mailbox is essentially "remember to look at it eventually".
The recommended convention is a two-step decay:

1. **Inactive for 12 months** → move the mailbox to `/Chapters/Dormant`. Login is suspended (so the address can't be used to send mail or sign in), but the data is preserved. The chapter still appears in the directory as dormant rather than vanishing.
2. **24 months in dormant** (so 36 months total of inactivity) → transfer any salvageable Drive content to the role mailbox, then delete the account. At this point the chapter has had three years to come back and hasn't; the mailbox is overhead with no audience.
3. **Chapter revives at any point before deletion** → reactivate from dormant. The data is intact, the address is unchanged, and the co-organisers pick up where the previous group left off.

The "how to suspend vs delete" mechanics — the literal click path, the data transfer dialog, when Google's grace period ends — already live in the [Admin Console runbook]({{% relref "admin-console#suspending-or-deleting-a-departing-account" %}}).
This page only adds the timing convention; don't duplicate the mechanics here.

TODO: maintainer to apply the lifecycle convention.
Today the call to suspend vs delete is ad-hoc; adopting the 12/24-month rule requires (a) agreeing it formally, (b) auditing the current chapter mailboxes for inactivity, and (c) moving any that already qualify into `/Chapters/Dormant` once the OU exists.

## Personal alias offboarding

Leadership and Global Team members rotate.
When they do, the current behaviour is to suspend the alias and forget about it.
That works until someone emails the departed person at their `@rladies.org` address two years later and gets silence rather than a useful redirect.

The recommended convention is a three-stage decay:

- **Day 0** — leadership departure confirmed. Move the alias from `/Global Team` to `/Alumni`. Set up forwarding (see below) and disable login.
- **Day 1 through Day 90** — incoming mail forwards automatically to the role mailbox, or to the person's personal address if they've explicitly opted in. Three months is long enough for stragglers to notice the address has changed and update their records.
- **Day 90** — delete the account. The Workspace seat returns to the pool and the address stops accepting mail.

Forwarding is configured per-user under **Apps → Google Workspace → Gmail → Routing**, or by signing in as the user one last time and setting forwarding from the mailbox's own settings.
The first approach is cleaner because it doesn't require touching the user's credentials.

Add this offboarding step to the Global Team offboarding checklist that lives in the [onboarding flow]({{% relref "/global-team/onboarding" %}}) — the same checklist that creates the alias is the right place to record how it gets retired.

TODO: maintainer to (a) create the `/Alumni` OU, (b) decide the default forwarding destination, and (c) add the offboarding step to the existing checklist.

## Shared Drives instead of My Drive

Anything important enough to outlive its author belongs in a Shared Drive, not a personal My Drive.
The failure mode the existing [Shared Drive page]({{% relref "shared-drive" %}}) describes — documents lost when a creator's account is suspended — only applies to My Drive content.
Shared Drives are owned by the tenant.

The recommended layout is one Shared Drive per Global Team function:

- Leadership
- Mentoring
- Finance
- Social Media
- Directory
- Onboarding

Each Drive gets the team lead as Manager and the team members as Contributor.
External collaborators get Viewer or Commenter scoped to specific files rather than full Drive membership.

The mechanics for granting access — the click path through **Manage members**, the choice of role — already live in the [Shared Drive page]({{% relref "shared-drive#granting-a-new-global-team-member-access" %}}); this page only adds the convention that each function gets its own Drive rather than sharing one big "Global Team" Drive.

TODO: maintainer to inventory the Shared Drives that currently exist and decide which need creating, renaming, or consolidating against the function list above.

## Email security at the DNS layer

`rladies.org` needs SPF, DKIM, and DMARC configured correctly or legitimate Workspace mail starts silently landing in recipients' spam folders.
The failure is silent because Google doesn't refuse to send the mail — the receiving server quietly downgrades it.

The DNS records themselves live at Cloudflare, since Cloudflare is the DNS host for `rladies.org`.
The Workspace side of DKIM (the key generation) happens in the admin console; the matching public key has to be published as a DNS record at Cloudflare for receiving servers to verify signatures.

To check the Workspace side:

1. Sign in to [admin.google.com](https://admin.google.com) as the role mailbox.
2. Open **Apps → Google Workspace → Gmail → Authenticate email**.
3. Confirm a DKIM key is generated for `rladies.org` and copy the TXT record value.
4. Verify that record exists at Cloudflare against the expected host (typically `google._domainkey.rladies.org`).

SPF and DMARC don't have a Workspace-side configuration — they're DNS-only.
The records live at Cloudflare; see the [Cloudflare]({{% relref "/global-team/infrastructure/cloudflare" %}}) page for how to reach the DNS dashboard and what the records should contain.

To verify the records are correct from the outside, point a public inspector at `rladies.org` — [mxtoolbox.com/dmarc](https://mxtoolbox.com/dmarc) or the equivalent on [dmarcian](https://dmarcian.com/dmarc-inspector/) will report all three checks (SPF pass/fail, DKIM signature alignment, DMARC policy and percentage) in one view.
For the Workspace-side DKIM key status, go to admin.google.com → **Apps → Google Workspace → Gmail → Authenticate email** and confirm the status reads _Authenticating email_ rather than _Not authenticating email_ or _Generating DKIM key_.
Both views should show green; if either is broken, mail starts landing in spam silently and nobody on the sending side gets an error.

TODO: maintainer to verify the current state of SPF, DKIM, and DMARC for `rladies.org` and document the actual record values.
A DMARC report aggregator (the free tier of a service like Postmark or dmarcian) is worth setting up so the maintainer sees the bounce/spam reports rather than relying on individuals to flag missing mail.

## Recovery drill

Every six months, a leadership member who is _not_ the primary holder of the 1Password vault should attempt to sign in to the role mailbox end-to-end — from opening 1Password, through the 2FA challenge, into the admin console.
If they can't, the recovery configuration is broken in a way no one would notice until the day it actually matters.

What running the drill looks like:

1. Open a fresh incognito (or private) window so no existing session masks a credential problem.
2. Navigate to [admin.google.com](https://admin.google.com) and enter the role mailbox.
3. Pull the password from the shared [1Password]({{% relref "/global-team/infrastructure/1password" %}}) vault and paste it in.
4. When prompted for 2FA, complete the challenge — either using the TOTP code from 1Password's authenticator field, or, if that fails, the SMS fallback to the recovery phone number on file.
5. Confirm the admin console loads with the expected privileges (Directory, Security, Apps all visible in the sidebar).
6. Sign out and close the incognito window.
7. If any step failed — wrong password, missing TOTP, no SMS arriving, console missing privileges — raise it on the leadership Slack channel that day, before anyone forgets which step broke.

This is not a fire drill to be done dramatically.
It's a calendar habit, scheduled alongside the [admin console review cadence]({{% relref "admin-console#review-cadence" %}}).
The output is either "yes, I got in" or "no, and here's where it failed", and the latter triggers a fix immediately rather than at the next emergency.

TODO: maintainer to schedule the first drill and document who runs it.
Pair the drill with the existing six-monthly admin review so it slots into a habit that already exists rather than competing for a new calendar slot.

## Migration plan

Moving from the current flat tenant to the structure above is a real project, not an afternoon's work.
The sequence below minimises disruption by changing the container before changing the contents, and by moving low-traffic accounts before high-traffic ones.

1. **Create the OUs** in the order `/Service accounts`, `/Global Team`, `/Chapters/Active`, `/Chapters/Dormant`, `/Alumni`. No users are moved yet; empty OUs are harmless. _Verify_: each OU appears under **Directory → Organizational Units** with the expected parent.
2. **Define policies per OU** in the admin console. Test each policy against a single test account before applying broadly. _Verify_: open the policy (e.g. 2-step verification) with the OU selected and confirm the panel reads "Settings for &lt;OU name&gt;" with the intended values.
3. **Move the role mailbox** into `/Service accounts` first. This is the highest-stakes account but also the one with the smallest blast radius if something goes wrong, because only the admin uses it. Run the recovery drill immediately after. _Verify_: open the user profile and confirm the **Organizational unit** field shows `/Service accounts`.
4. **Move Global Team personal aliases** into `/Global Team`. Tell people the move is happening so they can flag any unexpected login prompts that follow. _Verify_: spot-check two or three aliases in **Directory → Users** and confirm the OU column matches.
5. **Move chapter mailboxes** into `/Chapters/Active` last, in batches of ten or twenty. Warn co-organisers a week in advance so a sudden 2FA prompt doesn't read as a phishing attempt. _Verify_: filter the user list by OU `/Chapters/Active` and confirm the count matches the batch just moved.
6. **Stand up the Groups** in parallel with the OU moves. Groups don't depend on OU structure, so this work can happen any time. _Verify_: send a test message to each new group from an external address and confirm it lands in at least one member's inbox.
7. **Run the recovery drill** after each major move, not just at the end. Drift compounds; catch it early. _Verify_: the drill itself is the verification — the pass criterion is the incognito sign-in completing end-to-end.

TODO: this entire migration is the recommended next chunk of work, not work already done.
Track it as a single GitHub issue so the steps stay sequenced and the maintainer can check off each one as it completes.
