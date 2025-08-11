---
title: "Directory management"
linkTitle: "directory"
weight: 112
---


The R-Ladies directory source data is in a private repository containing potential personal information. 
If you have a local clone of this, please ensure it is stored securely.

## Delete Entries

To delete entries, please use the [Delete Entry](https://github.com/rladies/directory/actions/workflows/01-purge-init.yml) GitHub action. Follow these steps:

1. Click the "Run workflow" dropdown button.
2. Paste the filename of the file to delete.
3. Monitor the deletion process; associated image files will also be removed. 
4. A Pull Request (PR) will open to review the file deletion. 

Make sure to thoroughly check that only intended files are deleted. 
Once satisfied, merge the branch into the main branch.

New actions will validate the remaining JSON files.
The delete log is removed after 30 days, ensuring compliance with GDPR regulations. 
The website will update automatically once the deletion is complete and all checks pass.

## Add / Update Entries

Each directory entry must be reviewed and approved before it becomes visible on the [R-Ladies Directory website](https://rladies.org/directory/). 
The following steps detail how to review an automated Airtable Pull Request (PR) using R/RStudio. 

Note that:

1. Your R/RStudio session must have Git enabled.
2. You need access to the private [directory repository](https://github.com/rladies/directory).
3. Access to the directory form entries in Airtable is required.
4. Setup for receiving PR updates via email is necessary. 

For guidance on setting up R/RStudio for Git, refer to the [Happy Git and GitHub for the useR](https://happygitwithr.com/) ebook by Jennifer Bryan.

### Steps:

1. **Clone the Directory Repository**: 
   If it's your first time making directory changes, clone the directory repository onto your computer. 
   - Navigate to your preferred location (e.g., create "directory" in your Documents folder).
   - In R/RStudio, go to "File" > "New Project...", select "Version Control", and then "Git". 
   - Paste `https://github.com/rladies/directory` into the "Repository URL" and select your project folder. 
   - Click "Create Project", which might take a few minutes as it downloads the repository.

2. **Check the Current Branch**: 
   If you already have the directory files local, in the R/RStudio terminal, type:

   ```bash
   git branch
   ```
   
   Confirm "main" is starred and highlighted.

3. **Pull Recent Changes**: 
   Click the sideways "Git" logo and select "Pull" to update local changes. Repeat this step to ensure you’re up to date.

4. **Retrieve Recent PR**: 
   Navigate to the [directory repository](https://github.com/rladies/directory), go to the "Pull Requests" tab, and click the most recent PR. Note the PR ID (e.g., "airtable-update-XXXXXXX").

5. **Checkout the PR Branch**: 
   In R/RStudio, type:

   ```bash
   git checkout --track origin/airtable-update-XXXXXXX
   ```
   
   Replace "XXXXXXX" with the actual ID. Verify you’ve switched branches.

6. **Review Changes**:
   On GitHub, review the "Conversation" and "Files Changed" tabs. In R/RStudio, edit any necessary text files and check images (resize if necessary using your computer’s software). 

7. **Comment on Changes**: 
   If uncertain about something, leave comments on GitHub using the conversation bubble. Tag colleagues if needed with "@" and their GitHub ID.

8. **Commit Changes**: 
   Once you finish reviewing and making changes, click the sideways "Git" logo in R/RStudio, select "Commit...," and enter a commit message. Ensure you push your changes to GitHub.

9. **Confirm Changes on GitHub**: 
   Refresh the "Files changed" tab and assure all your changes are visible. Wait for automated checks to run before checking the preview.

10. **Approve the PR**: 
    After confirming everything looks good, click on the green "Review changes" button, select "Approve", and finalize.

11. **Merge the PR**: 
    Once all checks are completed, click "Squash and Merge" and then "Confirm Squash and Merge".

12. **Update Local Main Branch**: 
    In R/RStudio, switch back to the main branch with:

    ```bash
    git checkout main
    git pull
    ```

13. **Delete Local PR Branch**: 
    Remove local PR branch with:
    
    ```bash
    git branch -D airtable-update-XXXXXXX
    ```

14. **Update Airtable**: 
    Ensure to delete entries from Airtable that were added in the recent PR, except for the row labeled "#DONT-DELETE".

15. **Log Out of Airtable**.

16. **Check Directory Website**: 
    Optional: After a wait of 15-20 minutes, verify those changes appeared on the directory website.

17. **Congratulations!** 
    You've completed the process. Start again at step #2 the following Friday.

## Notes to Self

- **Entries from Males**: Consider how to handle entries submitted by males. Should we delete these files during editing/reviewing?
- **Previewing Updated Files**: Ensure a method to check updated previews once files are changed.
