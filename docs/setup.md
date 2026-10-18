# Setup Guide

## 1. Introduction

This guide explains how to import and configure the Telegram AI Personal Assistant workflows in an n8n instance.

The project contains one main workflow and five sub-workflows. The main workflow coordinates requests from Telegram, while the sub-workflows handle email, calendar, research, contacts, and X-post-related tasks.

Because n8n exports can contain instance-specific configuration, importing the JSON files is only the first step. Credentials, resource selections, and workflow references must be checked before testing.

## 2. Prerequisites

Before starting, ensure you have:

- Access to an n8n instance with permission to import and edit workflows.
- A Telegram bot configured for the assistant.
- The AI provider credentials required by the workflow.
- Access to the email service used by the Email workflow.
- Access to the calendar service used by the Calendar workflow.
- Access to the Google Sheets resources used by the Contacts and X Posts workflows.
- Access to any external research services required by the Research workflow.
- All six sanitized workflow JSON files.

The exact integrations depend on the current workflow exports.

## 3. Import the Sub-Workflows

Import the five sub-workflows into n8n first.

Recommended order:

1. Email Management Sub-Workflow.
2. Calendar Management Sub-Workflow.
3. Research Agent Sub-Workflow.
4. Contact Agent Sub-Workflow.
5. X Posts Agent Sub-Workflow.

In n8n, open the workflow editor and use the import-from-file option available in your version. Select the appropriate JSON file and save the imported workflow.

Review the nodes after import. If a node displays missing credentials, missing resources, or an invalid reference, resolve that configuration before relying on the workflow.

## 4. Import the Main Workflow

After importing the sub-workflows, import the Telegram AI Personal Assistant main workflow.

The main workflow is the user-facing entry point. It should be configured to receive Telegram requests and route tasks to the appropriate sub-workflows.

Do not activate it for live use until the credentials, resource selections, and sub-workflow references have been reviewed.

## 5. Configure Credentials

Open each workflow and review every node that requires authentication.

Depending on the workflow version, this may include:

- Telegram credentials.
- Email credentials.
- Calendar credentials.
- Google Sheets credentials.
- AI model credentials.
- Credentials for external research services, if required.

Select or create the required credentials using n8n's credential management interface.

Never place API keys, passwords, or access tokens directly into GitHub documentation or the exported workflow files.

## 6. Configure Resources

Review resource selections in each sub-workflow.

### Email Management

Confirm the intended email account and verify which email operations are configured.

### Calendar Management

Select the correct calendar. Verify the date, timezone, start time, and end time when testing event creation or updates.

### Research

Review the sources and nodes configured for research. The current export determines which services and operations are available.

### Contacts

Select the intended Google Sheets document and worksheet. Confirm the column names and expected data format.

### X Posts

Select the intended Google Sheets document and worksheet. Verify whether the workflow drafts, stores, or publishes content before enabling any live action.

## 7. Verify Sub-Workflow References

Open the main workflow and inspect each node that calls a sub-workflow.

Confirm that each node references the correct imported workflow:

- Email requests → Email Management.
- Calendar requests → Calendar Management.
- Research requests → Research Agent.
- Contact requests → Contact Agent.
- X-post requests → X Posts Agent.

The exact node names and input fields may differ. Confirm that the main workflow passes the fields expected by each sub-workflow and that the sub-workflow returns data in the format expected by the main workflow.

Workflow IDs from another n8n instance may not be valid in your instance. Reselect the appropriate imported workflow where necessary.

## 8. Test the Workflows

Test each sub-workflow separately before running an end-to-end Telegram request.

### Email test

Use a test mailbox or a safe draft operation where available. Confirm that the correct account and recipient are used.

### Calendar test

Create a clearly labeled test event. Verify the calendar, date, timezone, start time, and end time.

### Research test

Submit a simple research question and inspect the output and sources.

### Contacts test

Use a test spreadsheet or test row. Confirm that the intended data is read or written correctly.

### X Posts test

Use test content. Confirm the workflow's actual behavior before allowing any live publishing operation.

### Telegram test

Send a simple text request through Telegram. Confirm that the main workflow receives it, routes it correctly, and returns the result to the correct conversation.

If voice input is supported in the current export, test that path separately and verify that the transcription or voice-processing steps work as intended.

## 9. Troubleshooting

If a workflow fails, inspect the n8n execution and identify the first failing node.

Check the following:

- Required credentials are selected and authorized.
- Resource IDs and placeholders have been replaced.
- The correct calendar, spreadsheet, and worksheet are selected.
- The main workflow references the correct sub-workflows.
- Input and output fields match between workflows.
- Date and timezone settings are correct.
- Telegram is configured for the intended bot.
- The AI provider configuration is valid.

Prefer correcting the existing node configuration before adding new nodes or redesigning the workflow.

## 10. Activate for Use

Once individual tests pass:

1. Run an end-to-end test from Telegram.
2. Confirm that each requested task reaches the intended sub-workflow.
3. Confirm that results return to the user.
4. Review error handling.
5. Activate the required workflows according to their trigger configuration.
6. Monitor executions after activation.

## Important

Uploading JSON files to GitHub does not update a running n8n instance. If you modify the repository's workflow files, import or apply those changes to the target n8n instance separately.
