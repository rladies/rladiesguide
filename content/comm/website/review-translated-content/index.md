We've used DeepL to generate translations of our website content and have made them available in the [deepl](https://github.com/rladies/rladies.github.io/tree/deepl) branch of our repository. 
This branch is also available for [preview online](https://dmain-bmain-r413--rladies-dev.netlify.app/).

Now, we need your help to ensure these translations are accurate, natural-sounding, and align with the R-Ladies ethos.

## What We're Looking For in a Review

* **Accuracy of Meaning:** More than just translating words directly, we need to ensure the *intended meaning* of the original English content is accurately conveyed in the target language. Sometimes a direct word-for-word translation can sound awkward or miss the point. Think about the message we're trying to get across.
* **Clarity:** Is the translated text easy to understand and grammatically correct in the target language?
* **Natural Flow:** Does the language sound natural and not robotic or overly literal? Consider idiomatic expressions or common phrasing in the target language.
* **Inclusivity and Gender Neutrality:** Where possible and appropriate in the target language, please ensure the language is inclusive and gender-neutral in the prose surrounding the code.
* **Cultural Nuances:** Are there any cultural nuances that need to be considered or adjusted for the target audience?
* **Code-Specific Considerations:** When reviewing sections containing code, please pay attention to the following:
    * **Do Not Translate:** Function names, their arguments, and core programming keywords (like `function`, `if`, `else`, etc.) should **not** be translated. These are part of the programming language itself and need to remain in English for the code to be functional.
    * **Translate Where Appropriate:** Comments within the code should be translated to make the code's purpose and logic more accessible in the target language. Similarly, variable names can be translated if it significantly improves understanding for speakers of that language, while still maintaining clarity and consistency. Use your best judgment here – if a translated variable name becomes confusing or loses its technical meaning, it's best to leave it in English.

## How to Contribute A Review

You'll be working on a copy of our repository, called a "fork," to make your edits. Here's how to get started:

1. **Find content to review**
   * Go to the [preview online](https://dmain-bmain-r413--rladies-dev.netlify.app/), and in the footer select a language you speak and want to review.
   * Find a webpage you would like to provide a review on. They all have a purple notice at the top of the document, that notifies that this is an auto-translation and provides a link to the document you can edit. 
   * Check if someone is already working on a review of this particular page in our [translation tracking](https://github.com/orgs/rladies/projects/11) project.
      * If someone is already reviewing it, or we already have a first review on the content, please go to the relevant linked version of that and indicate in the comments that you would like to provide a second review of the content.
      * If the page is not listed in our tracking system, you can proceed to the next steps.

1.  **Fork the Repository:**
    * In the top right corner, click the "Fork" button. This creates a copy of the repository under your personal GitHub account.

2.  **Navigate to the "deepl" Branch:**
    * Once you're on your forked repository, switch to the "deepl" branch. You can usually do this by clicking on the branch name (likely "main") and selecting "deepl" from the dropdown menu.

3.  **Make Your Edits:**
    * Browse the files in the "deepl" branch. These contain the translated content, which may include code blocks.
    * Click on a file you want to review.
    * To make changes, click the "Edit this file" button (it looks like a pencil icon). This will allow you to modify the text directly in your browser.
    * Carefully review the translation, focusing on conveying the original meaning accurately and naturally in the surrounding text, and applying the code-specific translation guidelines. Make any necessary corrections or improvements based on all the criteria mentioned above.
    * When editing code blocks, be extra careful not to accidentally alter function names, arguments, or programming keywords.
    * Once you're satisfied with your changes in a file, scroll to the bottom and provide a descriptive commit message (e.g., "Review and edit French translation of page with code examples," "Improved clarity in Spanish code comments and variable names").
    * Click the "Commit changes" button.

4.  **Create a Pull Request (PR):**
    * After committing your changes to your fork, you'll see a prompt to "Compare & pull request" on your forked repository's page. Click this button.
    * On the "Open a pull request" page:
        * Ensure the **base repository** is the main R-Ladies website repository and the **base** branch is "deepl".
        * The **head repository** should be your fork and the **compare** branch should be the branch where you made your edits (it will likely be "main" unless you created a new branch in your fork).
        * Add a clear and concise title to your pull request (e.g., "Review of French translations including code sections," "Improvements to Spanish code comments and text").
        * In the description, provide a summary of the changes you've made, highlighting any adjustments to code comments or variable names and noting that you've avoided translating core code elements.
        * Click the "Create pull request" button.

### Reviewing Multiple Files

If you want to review several files, including those with code, you can do so all within your fork before creating a pull request:

1.  **Make Multiple Edits:** Follow steps 3 and 4 above for each file you want to review and commit the changes to your fork.
2.  **Create a Single Pull Request:** Once you've finished reviewing all the files, follow step 5 to create a single pull request. All the commits you made in your fork will be included in this one PR. This helps keep our main repository organized.

### Working on a Fork: What Does It Mean?

Think of a fork as your personal copy of the R-Ladies website repository. You can make any changes you like in your fork without affecting the original repository. When you create a pull request, you're essentially asking us to review the changes you made in your fork and merge them into the "deepl" branch of the main repository.

Your meticulous attention to both the prose and the code within our translated content is incredibly valuable in making our resources accessible to a global R-Ladies community. Thank you for your dedication!

## What happens after PR?

Once the PR is in place, team member from the @rladies/website team will have a look at what you have done.
Firstly, they will do some checks that makes sure that the general structure remains intact (all the correct yaml headers etc are still present).

Secondly, someone from @rladies/translation will have a look at the translated content and provide feedback if they are able to do so (i.e. we have someone on staff who speaks the language well enough). 
They will also try to find another person to have a second look at your review, so we can have the best translations we can. 
All changes to the content will be made in your fork where your translation review is, and will automatically be update the PR with all the changes you make.

Once everything looks good. the @rladies/website team member will remove the `translated: no` section of the yaml, indicating that this page has been reviewed, and will no longer have the auto-translate banner.