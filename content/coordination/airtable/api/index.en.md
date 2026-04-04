---
title: API access
menuTitle: "API access"
weight: 99
---

Airtable provides a powerful API that allows for programmatic access to your Airtable bases. 
This is particularly useful for automating tasks, integrating with other applications, or building custom workflows.

## Getting Started with the Airtable API

To use the Airtable API, follow these steps:
1. **Login with the R-Ladies Airtable account**: 
   - Use the credentials provided in the R-Ladies 1password vault "Shared".
3. **Get Your API Personal Acces Token**: 
   - Go to the [Airatable token creation page](https://airtable.com/create/tokens).
   - You will see several other tokens listed there, but you need to create a new one specifically for your project.
       - If there is another token name that seems appropriate, you can use that instead of creating a new one.
       - Search in 1password for the token name to see if it already exists.
       - Please do not alter or delete any existing tokens unless you are sure they are no longer needed by other Global team members.
   - Click on "Create a token" and follow the instructions.
4. **Save the token in 1password**:
   - Store the newly created token in the R-Ladies 1password vault for your team for future reference.
   - Ensure that you do not share this token publicly as it provides access to your Airtable bases.
4. **Find Your Base ID**:
   - Open your base in Airtable.
   - Click on the "Help" button in the top right corner and select "API documentation".
   - The Base ID will be displayed at the top of the documentation page.
5. **Use the API**:
   - You can now use the Airtable API with your Base ID and Personal Access Token.
   - The [API documentation](https://support.airtable.com/v1/docs/getting-started-with-airtables-web-api) provides detailed information on how to interact with your base, including creating, reading, updating, and deleting records.
   - There is also an R-package called [airtabler](https://github.com/bergant/airtabler) that provides an interface to the Airtable API, making it easier to work with Airtable data in R.

