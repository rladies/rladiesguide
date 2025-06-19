---
title: "Working with the website"
menuTitle: "Working with the website"
weight: 88
---

This guide will walk you through the process of contributing to a Hugo website using Git, tailored for those familiar with R.
We'll cover forking, cloning, adding, committing, pushing, and creating a Pull Request (PR).

## Understanding Forking and Cloning

1.  **Forking:** Think of forking as creating your _own personal copy_ of the website's code repository on a platform like GitHub. It allows you to make changes without directly affecting the original repository.

2.  **Cloning:** Once you have your forked copy online, **cloning** downloads it to your local computer so you can work on the files.

## Step-by-Step Guide

{usethis} has several handy functions for working with git repositories.
This forks and clones the repo, so you will end up having a version of the R-Ladies website in your GitHub, and a local copy of it to work on.

```r
install.packages("usethis")
usethis::create_from_github("rladies/rladies.github.io")
```

### If you prefer doing things yourself.

### Step 1: Fork the Repository

1.  Go to the repository page (e.g., `https://github.com/rladies/rladies.github.io`).
2.  Click the "Fork" button (usually in the top right).
3.  Choose your personal account to fork to.

You now have a copy like `your_username/rladies.github.io`.

### Step 2: Clone Your Fork

**Using the Terminal:**

1.  Open your terminal.
2.  Navigate to your desired directory: `cd ~/Documents/Projects`
3.  Copy the HTTPS URL of your fork (`https://github.com/your_username/rladies.github.io.git`).
4.  Clone the repository: `git clone https://github.com/your_username/rladies.github.io.git`
5.  Navigate into the cloned directory: `cd rladies.github.io`

### Step 3: Create a New Branch

**Using the Terminal:**

1.  Switch to the main branch: `git checkout main`
2.  Create and switch to a new branch: `git checkout -b your-new-branch-name`

**Using `usethis` in R:**

1.  Create and switch to a new branch: `usethis::pr_init("your-new-branch-name")`

### Step 5: Make Your Changes

Edit the Hugo markdown files using your preferred editor.

### Step 6: Add Your Changes

**Using the Terminal:**

```sh
git status
git add path/to/your/file.md
git commit -m "Your descriptive commit message"
```

**Using RStudio:**

Look in the git pane, and add and commit the files you have been working on.

**Step 8: Push Your Changes to Your Fork**

**Using the Terminal:**

1.  Push your branch: `git push origin your-new-branch-name`

**Using `usethis` in R:**

1.  Push your branch: `usethis::pr_push()`

**Step 9: Create a Pull Request (PR)**

1.  Go to your forked repository on GitHub.
2.  Click "Compare & pull request" or "Contribute".
3.  **Specify the Base Branch:** Choose the correct branch in the original repository (e.g., `main`, `develop`, `deepl`).
4.  Review your changes.
5.  Add a title and description to your PR.
6.  Click "Create pull request".

**Step 10: Keep Your Fork in Sync**

**Using the Terminal:**

1.  Switch to `main`: `git checkout main`
2.  Fetch upstream changes: `git fetch upstream`
3.  Merge upstream `main`: `git merge upstream/main`
4.  Push to your fork: `git push origin main`

**Using `usethis` in R:**

1.  Pull upstream: `usethis::pr_pull()`
2.  Push to your fork: `usethis::pr_push()`
