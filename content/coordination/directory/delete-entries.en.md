---
title: "Delete entires"
linkTitle: "directory"
weight: 13
---

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