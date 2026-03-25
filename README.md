# Project Automation Template

![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/Hardrivetech/project-automation-template/project-automation.yml?branch=main)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

This repository provides a ready-to-use GitHub Actions workflow for automating the creation of issues from a YAML roadmap file and adding them to a GitHub Projects (beta) board with a specified status (e.g., Backlog).

## Features
- Automatically creates issues from a YAML roadmap file
- Adds issues to your GitHub Projects (beta) board
- Sets the status of new issues (e.g., Backlog)
- Easy to configure and distribute


## Usage
1. **Fork or clone this repository** to your own GitHub account.
2. **Copy the workflow file** from `.github/workflows/project-automation.yml` into your target repository.
3. **Create your roadmap file** at `.github/vectora-roadmap.yml` (or update the path in the workflow if needed).
4. **Set up your GitHub Projects (beta) board** and retrieve the following IDs:
   - `PROJECT_ID`
   - `STATUS_FIELD_ID`
   - `BACKLOG_OPTION_ID`
   
   ### How to Retrieve Project IDs (Windows, Linux, Mac)
   You will need the [GitHub CLI](https://cli.github.com/) (`gh`) installed and authenticated. Replace `OWNER`, `REPO`, and `PROJECT_NUMBER` as needed.

   #### 1. Get the Project ID
   
   **All Platforms:**
   ```sh
   gh api graphql -f query='query { repository(owner: "OWNER", name: "REPO") { projectsV2(first: 10) { nodes { id title number } } } }'
   ```
   Find your project by title/number and copy the `id`.

   #### 2. Get the Status Field ID
   
   **All Platforms:**
   ```sh
   gh api graphql -f query='query { node(id: "<PROJECT_ID>") { ... on ProjectV2 { fields(first: 20) { nodes { id name } } } } }'
   ```
   Find the field with name `Status` and copy its `id`.

   #### 3. Get the Backlog Option ID
   
   **All Platforms:**
   ```sh
   gh api graphql -f query='query { node(id: "<STATUS_FIELD_ID>") { ... on ProjectV2SingleSelectField { options { id name } } } }'
   ```
   Find the option with name `Backlog` and copy its `id`.

   **Windows PowerShell:**
   - Use double quotes for the `-f query` argument and escape inner quotes with backticks:
     ```powershell
     gh api graphql -f query="query { node(id: `"<STATUS_FIELD_ID>`") { ... on ProjectV2SingleSelectField { options { id name } } } }"
     ```

   **Linux/Mac Bash:**
   - Use single or double quotes as shown in the examples above.

   > For more details, see the comments in the workflow file or visit the [GitHub CLI GraphQL docs](https://cli.github.com/manual/gh_api).

5. **Update the workflow file** with your IDs in the `env:` section.
6. **Create a secret** named `GH_TOKEN` with project write access in your repository settings.
7. **Push changes** to trigger the workflow.

## Customization
- You may freely modify and distribute this workflow.
- For detailed instructions on retrieving the required IDs, see the comments in the workflow file.

## License
MIT License. See LICENSE for details.
