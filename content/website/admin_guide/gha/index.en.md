---
title: GitHub Actions
weight: 3
menuTitle: GitHub Actions
---

## Website build action

The website is built using a GitHub Action that runs on every push to the main branch and on a daily schedule (to shuffle ranges like the directory).
Of note, the action pulls data from several repositories to populate the website, including:
- [rladies/directory](https://github.com/rladies/directory)
- [rladies/meetup_archive](https://github.com/rladies/meetup_archive)
- [rladies/awesome-rladies-blogs](https://github.com/rladies/awesome-rladies-blogs)

While the website repository contains mock data for these data, the action will always pull the latest data from the actual repositories to ensure that the website is up-to-date.


{{< mermaid >}}
graph TB

A[Checkout repository] --> B[Get Hugo version]
B --> C[Install cURL Headers]
C --> D[Setup R]
D --> E[Setup renv]
E --> F["Populate untranslated pages\n(scripts/missing_translations.R)"]

subgraph Site Data
F --> G["Get directory data\n(rladies/directory)"]
F --> H["Meetup\n(rladies/meetup_archive)"]
F --> I["Get blogs list\n(rladies/awesome-rladies-blogs)"]
G --> J["Clean cloned repos"]
J --> K["Merge chapter and meetup\n(scripts/get_chapters.R)"]
end

H --> J
I --> J
K --> L[Setup Hugo]
L --> M[Build]

M -->|Production| N[Deploy]

M -->|Preview| O[Install netlify cli]
O --> P[Deploy Netlify]
P --> Q["Notify PR about build"]
{{< /mermaid >}}

## Preview action

There are two ways to preview the website build:

1. **By GitHub Actions**: Builds the website either when a PR is created (or updated) or through a workflow dispatch event. 
The action will build the website and deploy it to a temporary URL, which can be used to preview the changes made in the PR. 
However, this action requires access to the repository secrets to deploy the preview, which are not available for forks. 
While the action is still triggered and the build is attempted, so we can catch build failures early, the deployment will be absent for forks.
1. **By Netlify**: Any PR from a fork to the website is rendered by Netlify, since the fork repository does not have access to our repository secrets (and thus the GitHub action builds will always fail). PRs from forks will need to be approved by a team member before the Netlify preview is available.

The GitHub action preview build is a fairly complicated workflow, which does the following:

{{< mermaid >}}
graph TD
    A[Workflow Dispatch] -->|Initialize Required Variables| E[Checkout Repository]
    E --> F[Set Env Parameters]
    
    F --> G(Install Libraries and Tools)
    G --> H[Clean Folders]
    H --> I{Directory Data Handling}
    
    I -->|Request main repo| J[Checkout DIRECTORY Repo]
    I -->|Request subset data | K[Download Directory Artifact]
    J --> L[Move DIRECTORY Data to correct folder]
    K --> L
    L --> M[Get Blogs List]
    M --> N[Fetch Meetup Data]
    N --> O[Clean and Organize Folders]
    
    O --> P[Setup Hugo]
    P --> Q[Build Site with Hugo]
    Q --> R{Is it a fork?}
    R -->|No| S[Deploy to Netlify]
    R -->|Yes| T[Skip Deployment]
    
    S --> U[Workflow Complete]
    T --> U

    U -->|Fail| W[Failure Notification]
    U -->|Success| V[Preview Notification]

{{< /mermaid >}}

When triggered, it has several inputs that are possible to set, which are mainly used to control what data it should fetch (for the directory and awesome-rladies-blogs, which come from a separate repository).
If the action is triggered by a workflow in another repository (e.g. from an action in the directory repo), it will also notify the source repository about the build status.
This is done so that data in these other repos are tested against the website build, and if there are issues, the other repository can be notified to fix them.

## Automatic merging of "pending" PRs

Once a post (news or blog) is finished, it should be labelled as "pending" on GitHub.
Any PR with a blog or news post that has the label "pending", will merge automatically into the main branch on the date specified in the content yaml.

{{< mermaid >}}

graph TD

    B[On: workflow_dispatch or daily schedule] --> C[Job: find_pending_prs];

    C --> C1[Step: Checkout Repository];
    C1 --> C2[Step: Find PRs with pending label];
    C2 -- outputs prs and process --> D{Condition: process == 'true'};

    D -- yes --> E[Job: process_prs];

    E --> E1[Step: Checkout Repository];
    E1 --> E2[Step: Process and Merge Qualifying PRs];
    E2 --> E3[Step: Trigger Website build];

    E3 --> F[End];
    D -- no --> F;

{{< /mermaid >}}

