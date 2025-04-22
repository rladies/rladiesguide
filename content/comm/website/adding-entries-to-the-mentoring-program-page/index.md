
We now use Hugo content files and page bundles to manage mentoring program entries. 
Here's how to add a new mentoring entry:

1.  **Using `blogdown::new_content()`:**

    * Open your R session and ensure your website project is the working directory.
    * Use the `blogdown::new_content()` function to create a new content file. You'll need to specify the path within the `content/mentoring` directory and the archetype you've created. Assuming your archetype is named `mentoring`, the command will look something like this:

        ```r
        blogdown::new_content(
          subdir = "mentoring",
          kind = "mentoring",
          title = "The R-Ladies Chapter" # Replace with the chapter name
        )
        ```

        * **`subdir = "mentoring"`**: This tells `blogdown` to create the new content within the `content/mentoring` directory.
        * **`kind = "mentoring"`**: This specifies that you want to use the `mentoring` archetype. This archetype should contain the necessary front matter fields for your mentoring entries.
        * **`title = "The Name of the New Mentor"`**: This will be used to generate the folder name for the page bundle and can also be used as the default title in the front matter. **Make sure to replace `"The Name of the New Mentor"` with the actual name of the mentor.**

2.  **Populating the Content File:**

    * The `blogdown::new_content()` function will create a new folder within `content/mentoring/`. The folder name will be based on the title you provided (e.g., `the-name-of-the-new-mentor`).
    * Inside this folder, you'll find an `index.md` (or `index.Rmd` if your archetype uses R Markdown). Open this file and fill in the necessary information for the new mentoring entry in the front matter (the section between the `---` delimiters). This will include details like the mentor's name, their bio, links, etc., as defined in your archetype.
    * You can also add any additional content or descriptions directly within the body of this `index.md` file.

Example from the Cotonou entry

```yaml
title: R-Ladies Cotonou
image: "cotonou.png"
summary: "I asked the speaker on that day to share her experience from choosing her topic to presenting! She ecstatically shared that and we immediately got volunteers for the next meetup"
date: "01-01-2019"
mentee: "Nadeja Sero"
mentor: ""
article: /blog/2021-02-04-cotonout_mentoring
```

The `summary` should be a quote from the Mentee about their experience, and the `article` would be an internal link to an associated blogpost. 
This last part is not mandatory, but it would be lovely for a blogpost written by the mentee and mentor about their experience with the program.

3.  **Adding the Image (Page Bundle):**

    * The new folder created by `blogdown::new_content()` is a Hugo page bundle. This means you can easily associate an image with their content.
    * Place the image file directly inside the newly created folder (e.g., `content/mentoring/the-name-of-the-new-post/`).
    * **Important:** The filename of the image should be consistent and referenced appropriately within your Hugo templates if needed (e.g., you might name it `featured.jpg` or the same as the folder name).

4.  **Committing and Pushing Your Changes:**

    * Once you've filled in the content and added the image, save the changes to the `index.md` file and the image file.
    * Commit these changes to your local Git repository with a descriptive message (e.g., "Add new mentoring entry for \[Chapter Name]").
    * Push your local branch to the remote GitHub repository.

5.  **Creating a Pull Request:**

    * Go to your repository on GitHub.
    * You should see a notification indicating your recently pushed branch. Click the "Compare & pull request" button.
    * Review your changes to ensure everything looks correct.
    * Add a clear and concise title and description for your pull request, summarizing the new mentoring entry.
    * Click the "Create pull request" button.

Once the pull request is created, the website team will review your changes and merge them into the main branch, making the new mentoring entry live on the website.
![Write a commit message, create a new branch, and propose the changes](https://github.com/rladies/website/blob/main/README_img/mentoring_edit4.png?raw=true)

### Further details on making a PR 

In the example images, a branch named `drmowinckels-patch-1` was made. 
Go to the main [repository page](https://github.com/rladies/website), and look for your new branch in the dropdown meny at the top left of the file contents table.

![Look for the branch name you made, and click it to enter it](https://github.com/rladies/website/blob/main/README_img/mentoring_edit5.png?raw=true)

The site should look more or less the same to you. 
Navigate to [content/activities/mentoring/img/](content/activities/mentoring/img/), and select `Add file` then `Upload files` 
![Choose to upload new files](https://github.com/rladies/website/blob/main/README_img/mentoring_edit6.png?raw=true)

Drag and drop the image for your entry, write a commit message, and make sure it is committed to the branch you are on (should be selected by default).
![Comment and propose changes to the branch you are on](https://github.com/rladies/website/blob/main/README_img/mentoring_edit7.png?raw=true)

Once this is done, you are redirected to the main repository page of your current branch, where a message that your branch has recently been pushed to. 
Click the `Compare & pull request` button that appears next to this message.
![Click on the 'Compare & pull request' button.](https://github.com/rladies/website/blob/main/README_img/mentoring_edit8.png?raw=true)

Change the head message for the pull request to clearly state what the proposed change is about. 
Click on the `Create pull request` button.
![Enter a meaningful short description of the changes and click 'Create pull request' button](https://github.com/rladies/website/blob/main/README_img/mentoring_edit9.png?raw=true).

Once this is done, someone on the website team will review your proposed changes and make sure that everything is working as expected.
If something needs changing, you will be contacted with information on what needs fixing.
Once everything looks fine, your changes will be merged into the main repository and will appear on the website.
