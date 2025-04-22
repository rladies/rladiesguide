---
title: "Contribute to the R-Ladies Blog"
menuTitle: "Blog"
weight: 1
---

- [Clone the project](#clone-the-project)
  - [Install the dependecies](#install-the-dependecies)
- [Write a new blog post](#write-a-new-blog-post)
  - [Header of your post](#header-of-your-post)
  - [After you write your post](#after-you-write-your-post)
- [PR your changes](#pr-your-changes)

<!-- /TOC -->

## Clone the project

Choose your own workflow! There are many ways you can fork and clone a project,
via the git terminal, the `{usethis}` package, the GitHub web UI, GitHub Desktop,
etc. Here we document two methods; however, you are welcome to use whichever with
which you are comfortable. We are happy to provide additional support or documentation
as needed.

You can proceed via either:

1. `git` commands in the terminal, or

2. through the `usethis` package

- An in-depth overview of this workflow is available at
  [Pull Request Flow with usethis](https://www.garrickadenbuie.com/blog/pull-request-flow-usethis/?interactive=1&steps=) by Garrick Aden-Buie.

### via git in the terminal

```sh
# Clone
git clone https://github.com/rladies/rladies.github.io.git rladies_website

# Enter
cd rladies_website/

# replace my_branch with any name you like
# will now be working on a copy of the main repo
git checkout -b my_branch
```

Open the folder in RStudio.

### via {usethis}

```r
# this forks and clones the repo
usethis::create_from_github("rladies/rladies.github.io")

# this creates a new branch for your post
usethis::pr_init("my_branch")
```

Open the folder in RStudio.

## Install the dependencies

We use [renv](https://rstudio.github.io/renv/articles/renv.html) to handle the website dependencies, and have several profiles for various tasks.
To contribute a blogpost, you will use the `default` branch.

```r
renv::activate(profile = "default")

# Restart your R session before doing this next step.
renv::restore()
```

Now renv should update the necessary dependencies for you to start working on your blogpost.

## Write a new blog post

Use the blogdown addin in RStudio to create a new post.
`Addin` -> `New post` -> `choose "blog" archetype`

Fill inn the fields with the relevant information.

We are going to follow a few rules to set the header of the post to set posts easily discoverable.

- **Title**: Post title. It is the main feature, it shows in the list and the post page.
- **Author**: Post author. It is not visible in the moment. We will work to make it visible and to show in a menu.
- **Date**: Post date. Same as author. The date _must_ be static, nothing like `r Sys.Date()`, otherwise everytime the blog is generate a new date is set in the blogpost. The **format must be YYYY-mm-dd**.
- **Description**: Post subtitle. As an example, we used it to set the title of the post series of the 2018 IWD project. It shows in the posts list and in the post page.
- **Tags**: Post tags. They should include meaningful information like date if it is a recurrent project (because dates are not shown anywhere for now). 4 or 5 tags is a good number. [Tags](https://rladies.org/tags/).
- **Categories**: Post categories. Like tags but the theme is more general. They are not visible right now. [Categories](https://rladies.org/categories/).

All of the information will be shown in the post yaml, and can also be edited later.

Example:

```yaml
---
title: "1. Behind the scenes of R-Ladies IWD2018 Twitter action!"
author: "R-Ladies"
date: "2018-03-26"
description: "Part 1: Ideation and Creation!"
tags:
  - iwd
  - twitter
categories:
  - R-Ladies
---
```

Your post will be created in a folder within `content/blog` and you can add all the files you need for you post here.
Any images, data files or other things you need to include in your post, add them directly in the folder.

As you write you post, remember to `knit` your post so have a look at how it looks like!
The blogdown site will use the markdown file created from your knitted `Rmd` on the site, not your `Rmd` itself.
You can preview the entire site with your post with `blogdown::serve_site()`.

### After you write your post

Once your post looks as you want it to on your local machine, it's time to push the post
up to the main repository for review.

### via git in the terminal

```sh
# Add the post
git add content/post/yyyy-mm-dd-your-post-title

# Add a commit message to the post
git commit -m "my commit message"

# Push changes online (replace my_branch with your branch name)
git push --set-upstream origin my_branch
```

### via {usethis}

In the RStudio git pane, check your post folder and commit with a message.

Then push with

```r
usethis::pr_push()
```

## PR your changes

Once you have pushed your changes online, make a [Pull request (PR)](https://github.com/rladies/blog/pulls) to the main branch.
If you are not a member of the R-Ladies Global team, you will be working on a "fork" of the repository.

When the PR is open ([see here](https://github.com/rladies/blog/pulls)), a person from the blog or website team will be assigned as your reviewer and will take you through the remaining process.

When working on a fork, the build of the website will run, but no preview will be created.

### Website/Blog administration take-over

After the PR is received, someone in the @rladies/blog or @rladies/website team will have a look at your post.
They will request changes through GitHub code review, where you will need to adapt or accept the changes they propose to your document.

Once the team is satisfied with the post, they will [create a new branch from the main repository](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-and-deleting-branches-within-your-repository#creating-a-branch) and call it something like "fork\_[newpostname]", and then [switch your PR's base branch](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/changing-the-base-branch-of-a-pull-request) to the newly made fork rather than `main`.
They will then merge your PR into this branch, and wait for a preview to be generated.

Ideally, at this point, only other changes that might need to be done are editorial and up to the @rladies/blog or @rladies/website team to fix.
They will ask for you assistance directly if anything else is needed.

## Clarification

If anything is unclear in these guidelines, please submit [an issue](https://github.com/rladies/blog/issues), so that we can assist you and improve our documentation.
