---
title: "Deleting Entries"
weight: 13
---

In our project, maintaining a clean repository history is crucial, particularly because the R-Ladies directory contains personal information. It is essential to ensure that any entries that need to be removed are purged from both the current state of the repository and its history. This practice is vital to prevent any accidental exposure of sensitive information, uphold data integrity, and comply with data protection regulations.

The process for purging entries is managed through a series of automated GitHub Actions. These actions are designed to effectively delete specified entries while ensuring that both the current repository state and its history are updated accordingly.

## Step-by-Step Purging Process

Every Friday, the GitHub Action for Airtable updates runs automatically. This action checks for any entries that require purging before new entries are added to the repository. 

It is imperative to remove entries prior to any additions or updates to maintain a clean repository and avoid potential errors stemming from historical mismatches.

The purging process begins by evaluating whether there are entries that need to be removed.

## Check for Entries to Purge

The initial step in the process is verifying if there are any entries that require deletion. 
This is done through a query to an Airtable base that maintains a comprehensive list of entries.

If any records in Airtable indicate a delete request, the system triggers the corresponding purging process.

## Initiate the Deletion Process

The action for purging entries can be initiated manually or automatically. 
When triggered, it allows users to specify which file names they wish to delete.

In cases where someone contacts the team directly or needs to expedite the deletion, this manual initiation complements the automated checks conducted every Friday.

To initiate a manual deletion, this can be done in the GitHub Actions tab of the repository.

**Steps**:

1. Navigate to the **Actions** tab in the repository.
2. Select the **Purge Entries** workflow.
3. Click on the **Run workflow** button.
4. Specify the file names to delete in the input field. (e.g. hannah-mason.json,rachel-smith.json)
5. Click the **Run workflow** button to start the deletion process.


## Delete Entries from Current History

The action then scans for the specified files and deletes identified entries. 
It verifies the appropriateness of the files before proceeding with the removal.

## Purge Deleted Files from Git History

To ensure that the deleted files do not exist in the history of the repository, the action utilizes specialized tools to permanently erase these entries from past commits.
This means that the history of the repository is scrubbed clean of any traces of the deleted files, and will the state of the repository will no longer be compatible with previous versions.

## Push Changes to GitHub

After the purging actions are complete, a new branch reflecting the updated state is created, ensuring that all changes are securely applied.

## Create an Issue for Review

As a measure of accountability, an issue is automatically generated to inform the team about the deletions.
This issue prompts team members to review the changes for validation before forcing the changes into the main branch.

## Force Push Purged Entries into Main

The created issue should contain necessary information to review the changes made, and steps that need to be taken to ensure that the repository is updated correctly.
Closing the issue as is, will let the repo remain in its current state, not applying the changes made by the purging action.
By applying the label `force-push`, the team indicates that the changes should be force pushed to the main branch.
Closing the issue with this label will trigger a force push of the purged entries to the main branch.
This will completely rewrite the history of the main branch, removing the deleted entries from all previous commits.

## Delete Airtable Entries

Once the issue is closed with the `force-push` label, the action will also delete the corresponding entries from Airtable that triggered the purging process.

## Trigger New Updates

Following the force push, a new Airtable update is triggered to refresh the data within the repository based on the recent changes made, and a PR (Pull Request) is created to reflect these updates.

**Of note**: if the entry that triggered this whole process is still present in the Airtable base, it will trigger another history rewrite. 
No other airtable update will be triggered until the entry is removed from Airtable.

## Conclusion

This systematic approach to purging entries is an essential aspect of our data management strategy.
By utilizing automated processes through GitHub Actions, we ensure that sensitive information is handled securely and that the integrity of our repository is maintained.

By adhering to these steps, our team can effectively manage entries within the repository, reducing the risk of unintentional data exposure and errors from historical records.


{{% mermaid %}}
graph TD;

    A[Start Purging Process] --> B[Check for Entries to Purge]
    
    B -->|Entries Identified| C[Initiate Deletion Process]
    
    B -->|No Entries to Purge| Z[End Process]

    C --> D[Delete Entries from Current History]
    
    D --> E[Purge Deleted Files from Git History]
    
    E --> F[Push Changes to the Repository]
    
    F --> G[Create an Issue for Review]
    
    G -->|Issue Closed without Labels| H[End Process]
    
    G -->|Issue Closed with 'force-push'| I[Force Push Purged Entries into Main]

    I --> L[Delete Airtable Entries]
    
    L --> J[Trigger New Updates]
    
    J --> K[End Process]
    
    H -->|If Entry Still in Airtable| B
{{% /mermaid %}}
