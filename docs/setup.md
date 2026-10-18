# Setup Guide

This guide explains how to import and configure the Telegram AI Personal Assistant in your own n8n instance.

The project has one main workflow and five sub-workflows (all in [`workflows/`](../workflows/)). The main workflow receives Telegram messages and calls the sub-workflows as tools. The JSON files are sanitized exports: credentials, resource IDs, and workflow references are placeholders that you must replace in your own instance. See the [Architecture](architecture.md) document for how the pieces fit together.

## 1. Prerequisites

| You need | Used for |
|---|---|
| An n8n instance (with **Data Tables** available) | Running the workflows and storing the Telegram allow-list |
| The **ElevenLabs community node** (`@elevenlabs/n8n-nodes-elevenlabs`) installed in n8n | Voice-message transcription and spoken replies |
| A **Telegram bot token** (create a bot with [@BotFather](https://t.me/BotFather)) | Receiving and sending Telegram messages |
| A **Google Gemini (PaLM) API key** | The AI model used by every agent (`models/gemini-3.5-flash-lite` in the exports) |
| A **Gmail** credential | Email Management sub-workflow |
| A **Google Calendar** credential and a calendar ID | Calendar Management sub-workflow |
| A **Google service account** with access to two Google Sheets | Contact Agent and X Posts Agent sub-workflows |
| An **ElevenLabs API key** | Speech-to-text and text-to-speech in the main workflow |

Research needs no credentials: it uses the Wikipedia and Hacker News nodes.

## 2. Prepare your data

### Google Sheets

Create two spreadsheets and share each one with your service account's email address (Editor access).

| Spreadsheet | Worksheet | Columns (row 1) |
|---|---|---|
| Contacts | `Contacts` | `Name`, `Email`, `Phone` |
| X Posts | `Twitter` | `Date`, `Theme`, `Content` |

The Contact workflow matches existing rows on **Phone**, so contacts are updated by phone number.

### Telegram allow-list (n8n Data Table)

The assistant is private. Only chats listed in a Data Table can use it.

1. In n8n, create a Data Table with two text columns: `chat_id` and `status`.
2. Add a row for yourself: `chat_id` = your Telegram chat ID, `status` = `active`.
3. Anyone not in this table receives "This assistant is private. You are not authorized to use it."

To find your chat ID, send any message to the bot after the workflow is running, open that execution in n8n, and read `message.chat.id` from the Telegram Trigger output.

## 3. Import the workflows

Import the sub-workflows first, then the main workflow, so the main workflow can reference them.

1. `workflows/email-management-sub-workflow.json`
2. `workflows/calendar-management-sub-workflow.json`
3. `workflows/research-agent-sub-workflow.json`
4. `workflows/contact-agent-sub-workflow.json`
5. `workflows/x-posts-agent-sub-workflow.json`
6. `workflows/telegram-ai-personal-assistant.json` (main)

In n8n, create a new workflow and use **Import from file** (the menu in the top-right of the editor). Save each workflow after importing it. Every export has `active: false`, so nothing runs until you activate it.

## 4. Replace the placeholders

Search each workflow for the values below and replace them.

| Placeholder | Where | Replace with |
|---|---|---|
| `YOUR_GOOGLE_SHEET_ID` | Contact Agent (2 nodes), X Posts Agent (1 node) | The spreadsheet ID from the sheet's URL |
| `YOUR_SHEET_GID` | Same nodes | The worksheet's `gid` (the number after `gid=` in the URL) |
| `YOUR_GOOGLE_CALENDAR_ID` | Calendar Management (6 nodes) | Your calendar ID |
| `YOUR_CREDENTIAL_ID` | X Posts Agent's Gemini model node (the other workflows have no credential reference exported) | Select your own Gemini credential |
| `RESELECT_SUBWORKFLOW_AFTER_IMPORT` | Main workflow, 5 tool nodes | Select the matching imported sub-workflow |
| `RESELECT_OR_CREATE_DATATABLE_AFTER_IMPORT` | Main workflow, authorization node | Select the Data Table from section 2 |
| `YOUR_NAME` | Main workflow, AI agent system prompt | The name to sign outgoing emails with |

## 5. Connect credentials

Open every workflow and fix any node that shows a missing-credential warning:

- **Main workflow:** Telegram, Gemini, and ElevenLabs (speech-to-text and text-to-speech).
- **Email Management:** Gmail (all ten Gmail tool nodes) and Gemini.
- **Calendar Management:** Google Calendar and Gemini.
- **Contact Agent / X Posts Agent:** Google Sheets service account (and Gemini for X Posts).
- **Research Agent:** Gemini only.

Never paste keys or tokens into node parameters, prompts, or this repository. Use n8n's credential manager.

## 6. Check the sub-workflow references

In the main workflow, the five tool nodes must point to the right imported workflows:

| Tool node | Sub-workflow |
|---|---|
| `Gmail_Tool1` | Email Management |
| `Calendar_Tool1` | Calendar Management |
| `search_agent1` | Research Agent |
| `contact_agent1` | Contact Agent |
| `x_posts1` | X Posts Agent |

Workflow IDs from another n8n instance are not valid in yours, which is why these are placeholders.

## 7. Review settings that are specific to the original author

- **Timezone:** the Calendar agent's prompt tells it to use RFC3339 timestamps with the **Africa/Lagos (UTC+1)** offset. Change this to your own timezone.
- **Attendee invitations:** `createEventWithAttendee` is documented in the prompt as using a Google service-account credential, which Google rejects for attendee invites unless Domain-Wide Delegation is configured. Use a Google Calendar OAuth2 credential for that node if you need invitations.
- **Voice:** the text-to-speech node uses an ElevenLabs voice ID and the `eleven_turbo_v2_5` model. Pick any voice available to your account.
- **Language:** speech-to-text and text-to-speech are set to English (`en`).

## 8. Test

Test the sub-workflows individually before the end-to-end flow, using test data:

| Test | What to check |
|---|---|
| Email | Use a test mailbox. Search and draft first; confirm nothing is sent without approval. |
| Calendar | Create a clearly labeled test event. Verify the calendar, date, timezone, start, and end. |
| Research | Ask a simple question; confirm the answer cites Wikipedia or Hacker News results. |
| Contacts | Save and look up a test contact in a test spreadsheet. |
| X Posts | Ask for a draft; confirm a row appears in the sheet. The workflow only saves drafts and never publishes to X. |
| Telegram text | Message the bot from an allow-listed account. |
| Telegram voice | Send a voice message and confirm you get a voice reply. |
| Authorization | Message the bot from a non-listed account and confirm it is refused. |
| Approval gate | Ask it to send an email or create an event. Telegram should show an **Approve** button first, and nothing should happen if you decline or wait 30 minutes. |

## 9. Activate

When the tests pass, activate the main workflow (the Telegram Trigger must be active) and the sub-workflows according to your n8n version, then monitor the first executions.

## Troubleshooting

If something fails, open the execution in n8n and find the first failing node. Typical causes:

- A placeholder from section 4 was not replaced.
- A credential is missing or not authorized for the right account.
- The spreadsheet or calendar is not shared with the service account.
- A tool node still points to a sub-workflow ID from another instance.
- The chat ID is missing from the Data Table, or its `status` is not `active`.
- The ElevenLabs community node is not installed.

## Important

Uploading JSON files to GitHub does not update a running n8n instance. If you change the files in this repository, import or apply the changes in n8n separately.
