# n8n-workflow-autopublisher-11tui

## Overview

This workflow monitors a Google Drive folder for new n8n workflow JSON files. Upon detection, it downloads each file, uses Google Gemini to analyze the workflow and generate documentation details (like features, prerequisites, and usage steps). It then creates a new public GitHub repository, populates it with the original workflow JSON, a README.md, and a SETUP.md generated from the AI-extracted information, and finally records the processed file in a Google Sheet.

## Features

- Monitors a Google Drive folder for new n8n workflow JSON files.
- Identifies new workflow files by comparing against a Google Sheet of processed items.
- Processes multiple new workflow files sequentially.
- Extracts workflow content and sanitizes file names for repository creation.
- Leverages AI (Google Gemini) to automatically analyze n8n workflows and extract key documentation details.
- Dynamically generates comprehensive `README.md` and `SETUP.md` files for new repositories.
- Creates new public GitHub repositories.
- Pushes the n8n workflow JSON, generated `README.md`, and `SETUP.md` to the newly created GitHub repository.
- Records processed workflow files (name, ID, repo URL) in a Google Sheet to prevent re-processing.

## Services Used

- Google Drive
- Google Sheets
- Google Gemini API
- GitHub API

## Trigger

Runs every 15 minutes via a Schedule Trigger, checking for new workflow files.

## Prerequisites

- A Google Drive folder containing n8n workflow JSON files to be processed.
- A Google Sheet with document ID '15c4tbActQoRMffQ6Z94lDarnrYJoS2R75KGTICUUTUM' and a sheet named 'processed' (or gid=0) containing at least a 'file_id' column header.
- A GitHub account where new repositories will be created.
- Access to Google Gemini API (via a Google Palm API credential).

## Credentials

- Google Drive OAuth2 API (for listing and downloading files).
- Google Sheets OAuth2 API (for reading and appending data).
- HTTP Header Auth (for GitHub API interactions).
- Google Palm API (for Google Gemini model interaction).

## Configuration

1. Configure Google Drive OAuth2 API credential for file access.
2. Configure Google Sheets OAuth2 API credential for sheet operations.
3. Configure HTTP Header Auth credential for GitHub with a Personal Access Token (PAT) having `repo` scope.
4. Configure Google Palm API credential for Gemini model access.
5. Update the 'List Drive Folder' node with the correct Google Drive folder ID to monitor.
6. Ensure the 'Read Processed Sheet' and 'Mark File as Processed' nodes point to the correct Google Sheet document ID and sheet name.

## Usage

1. Activate the workflow in n8n.
2. Place new n8n workflow JSON files into the designated Google Drive folder.
3. The workflow will automatically detect, process, and publish these files to GitHub during its scheduled runs.
4. Review the generated GitHub repositories for the workflow JSON and documentation (README.md, SETUP.md).

## Troubleshooting

- If 'Gemini output is not valid JSON' error occurs, check the Gemini prompt and its API response structure for unexpected formatting.
- Ensure all required credentials for Google Drive, Google Sheets, GitHub, and Gemini API are correctly set up and have the necessary permissions.
- Verify that the Google Drive folder ID and Google Sheet IDs are accurate.
- Check n8n execution logs for any specific API errors from connected services.

## Security Notes

- The GitHub Personal Access Token (PAT) used must have sufficient permissions (e.g., `repo` scope) to create repositories and push content. Store this PAT securely in n8n credentials.
- Be mindful of the content within workflows sent to Google Gemini for analysis, especially if they contain sensitive data.
- New GitHub repositories are created as *public* by default. Modify the 'Create GitHub Repo' node if private repositories are desired.
