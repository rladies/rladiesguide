---
title: "Updating entires"
weight: 2
---


Each directory entry must be reviewed and approved before it becomes visible on the [R-Ladies Directory website](https://rladies.org/directory/). 

On a weekly basis, an automated process runs to pull new entries from the R-Ladies Airtable form and create a Pull Request (PR) in the [R-Ladies Directory repository](https://github.com/rladies/director) (the repo is private and only accessible to R-Ladies Global website and directory teams). This typically happens on Thursday evening Eastern Time (ET) or Friday morning Central Europe Time (CET).
The PR will auto-assign the PR to a directory team member, who will then review the entries and merge them into the main branch if everything looks good.

The PR contains the following:
- New entries from the Airtable form.
- Changes to existing entries.

## Reviewing PR on GitHub

There are a couple things you cannot change in the GitHub review pane:

- cannot alter file names for images
- cannot resize images

To review the PR on GitHub, follow these steps:

1. **Go to the R-Ladies Directory Repository**: 
   Navigate to the [R-Ladies Directory repository](https://github.com/rladies/director).
2. **Go to the Pull Requests Tab**:
   Click on the "Pull Requests" tab to see the list of open PRs.
3. **Select the Latest PR**:
   Click on the most recent PR, which should be titled "Update directory entries from Airtable".
4. **Review the Conversation**:
   Read through the conversation tab to see any comments or discussions related to the PR.
   In particular look for the bot comment that provides a link to a preview of _only_ the new entries added to the directory on the R-Ladies website. This is useful for quickly checking the new entries without having to go through all the files changed in the PR. It's also a great place to check whether images need to be cropped.
5. **Read through the PR Description**:
   The PR description contains a set of tasks to perform in relation to checking the new entries and existing entries. These were developed based on previous PR review experiences and common issues. They're intended to help you with the review process.
6. **Start the Review**:
   Click on the "Add Your Review" button to start your review. You can leave comments, approve the PR, or request changes. This will take you to the "Files Changed" tab.
7. **Review Files Changed**:
   You can also click directly on the "Files Changed" tab to see the changes made in the PR. This will show you the new entries added and any existing entries that have been modified.
8. **Check for New Entries**:
   Look for new entries that have been added to the directory. These will be in the form of new files or modifications to existing files.
9. **Check for Existing Entries**:
   Review any existing entries that have been modified. Ensure that the changes are appropriate and do not contain any errors or issues.
10. **Comment on Changes**:
   If you have any questions or concerns about the changes, leave comments on the PR. You can tag other R-Ladies members if you need their input or assistance.
11. **Alter text content (remotely)**:
   If you need to make changes to the text content of any entries, you can do this directly in the GitHub interface. 
   - Click on the plus icon next to the file name to edit the file.
   - On the line you want to change, click the pluss icon to edit the line.
   - To make a suggestion, click on the "Add a suggestion" button (a rectangle with + - on top of each other).
   - the comment box will change to include lines like this:

     ```suggestion
     <original line>
     ```
   - Alter the line to what you want it to be, and then click "Add suggestion".
   - This will create a suggestion that can be reviewed and approved later.
12. **Alter file names and crop immages (locally)**:
  If you need to make changes to the file names (i.e., because they contain a special character or extra blank space), you will have to make a copy of the PR to your local computer using Git. Follow the "Thorough local review process" below for more details.
13. **Approve the PR**:
   Once you have reviewed the changes and are satisfied with them, click on the green "Review changes" button. Select "Approve" and add any comments if necessary.
14. **Merge the PR**:
    After approving the PR, click on the "Squash and Merge" button to merge the changes into the main branch. Confirm the merge by clicking "Confirm Squash and Merge".
15. **Update Airtable**:
    Ensure to delete entries from Airtable that were added in the recent PR, except for the row labeled "#DONT-DELETE".
16. **Log Out of Airtable**.
17. **Congratulations!**:
    You've completed the process! Start again at step #1 the following week.

## Thorough local review process

The following steps detail how to review an automated Airtable Pull Request (PR) using R/RStudio. 

1. Your R/RStudio session must have Git enabled.
2. You need access to the private [directory repository](https://github.com/rladies/directory).
3. You need access to the directory form entries in Airtable.
4. You need access to the R-Ladies 1password account to be able to log into Airtable.
5. You are setup to receive PR updates via email.

For guidance on setting up R/RStudio for Git, refer to the [Happy Git and GitHub for the useR](https://happygitwithr.com/) ebook by Jennifer Bryan.

### Steps:

1. **Clone the Directory Repository**: 
   If it's your first time making directory changes, clone the directory repository onto your computer. 
   - Navigate to your preferred location (e.g., create "directory" in your Documents folder).
   - In R/RStudio, go to "File" > "New Project...", select "Version Control", and then "Git". 
   - Paste `https://github.com/rladies/directory` into the "Repository URL" and select your project folder. 
   - Click "Create Project", which might take a few minutes as it downloads the repository.

2. **Check the Current Branch**: 
   If you already have the directory files local, in the **R/RStudio terminal window**, type:

   ```bash
   git branch
   ```
   
   Confirm "main" is starred and highlighted.

3. **Pull Recent Changes**: 
   In your **RStudio** session, Click the sideways "Git" logo and select "Pull" to update local changes. Repeat this step to ensure you’re up to date.

4. **Retrieve Recent PR**: 
   Navigate to the [directory repository](https://github.com/rladies/directory), go to the "Pull Requests" tab, and click the most recent PR. Note the PR identifier (e.g., "airtable-update-XXXXXXX").

5. **Checkout the PR Branch**: 
   In the **R/RStudio terminal window**, type:

   ```bash
   git checkout --track origin/airtable-update-XXXXXXX
   ```
   
   Replace "XXXXXXX" with the actual ID. Verify you’ve switched branches.

6. **Review Changes**:
   On GitHub, review the "Conversation" and "Files Changed" tabs. In R/RStudio, edit any necessary text files and check images (resize if necessary using your computer’s software). 

7. **Comment on Changes**: 
   If uncertain about something, leave comments on GitHub using the conversation bubble. Tag colleagues if needed with "@" and their GitHub ID.

8. **Commit Changes**: 
   Once you finish reviewing and making changes, click the sideways "Git" logo in **R/RStudio**, select "Commit...," and enter a commit message. Ensure you push your changes to GitHub.

9. **Confirm Changes on GitHub**: 
   Refresh the "Files changed" tab and assure all your changes are visible. Wait for automated checks to run before checking the preview.

10. **Approve the PR**: 
    After confirming everything looks good, click on the green "Review changes" button, select "Approve", and finalize.

11. **Merge the PR**: 
    Once all checks are completed, click "Squash and Merge" and then "Confirm Squash and Merge".

12. **Update Local Main Branch**: 
    In the **R/RStudio terminal window**, switch back to the main branch with:

    ```bash
    git checkout main
    git pull
    ```

13. **Delete Local PR Branch**: 
     In the **R/RStudio terminal window**, remove your local PR branch with:
    
    ```bash
    git branch -D airtable-update-XXXXXXX
    ```

14. **Update Airtable**: 
    Ensure to delete entries from Airtable that were added in the recent PR, except for the row labeled "#DONT-DELETE".

15. **Log Out of Airtable**.

16. **Check Directory Website**: 
    Optional: After a wait of 15-20 minutes, verify those changes appeared on the directory website.

17. **Congratulations!** 
    You've completed the process! Start again at step #2 the following week.
