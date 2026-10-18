# Telegram AI Personal Assistant

An AI-powered personal assistant built with **n8n, Telegram, and AI integrations**. The project allows users to interact with an assistant through Telegram and delegate tasks to specialized automation workflows.

The system uses a main workflow to coordinate user requests and five sub-workflows to organize different responsibilities.

## Project Overview

The Telegram AI Personal Assistant is designed to simplify everyday tasks by connecting a conversational interface to automation workflows.

Instead of manually switching between multiple applications, a user can send a request through Telegram. The main workflow processes the request and routes it to the appropriate sub-workflow.

### Key Features

- **Telegram interface:** Text and voice messages. Voice is transcribed with ElevenLabs, and voice requests get a spoken reply.
- **Private by design:** Only Telegram chats on an allow-list (an n8n Data Table) can use the bot.
- **Human approval:** Sending email, changing the calendar, and saving contacts require an **Approve** tap in Telegram first.
- **Email management (Gmail):** Search, summarize, prioritize, draft, send, reply, label, and mark read or unread.
- **Calendar management (Google Calendar):** View events, check availability, and create, update, or delete events.
- **Research:** Answers questions using Wikipedia and Hacker News.
- **Contacts (Google Sheets):** Look up and save contacts.
- **X (Twitter) post drafts (Google Sheets):** Writes post drafts and saves them to a sheet. It does not publish to X.
- **Modular architecture:** A main coordinating agent plus five sub-workflows that can be tested and maintained separately.

## Workflow Structure

The project consists of six n8n workflows:

| Workflow | Responsibility |
|---|---|
| Telegram AI Personal Assistant | Main entry point and request coordinator |
| Email Management Sub-Workflow | Email-related tasks |
| Calendar Management Sub-Workflow | Calendar-related tasks |
| Research Agent Sub-Workflow | Research and information gathering |
| Contact Agent Sub-Workflow | Contact-related operations |
| X Posts Agent Sub-Workflow | X-post-related operations |

## Architecture

The general request flow is:

1. The user sends a text or voice message through Telegram.
2. The main workflow checks that the chat is on the allow-list. Anyone else is refused.
3. Voice messages are transcribed; unsupported message types get a short reply.
4. A single AI agent decides which tools it needs and calls the matching sub-workflows (email, calendar, research, contacts, X drafts).
5. For consequential actions, the agent asks for approval in Telegram first.
6. The assistant replies in Telegram, with a voice message if the request was a voice message.

## Technology Stack

- **n8n:** Workflow automation and orchestration (including Data Tables).
- **Telegram:** User-facing messaging interface.
- **Google Gemini:** The AI model behind every agent.
- **ElevenLabs:** Speech-to-text and text-to-speech for voice messages.
- **Gmail and Google Calendar:** Email and calendar operations.
- **Google Sheets:** Contact and X-post draft storage.
- **Wikipedia and Hacker News:** Research sources.

See [docs/architecture.md](docs/architecture.md) for diagrams and details.

## Repository Structure

```text
Telegram-AI-Personal-Assistant/
├── README.md
├── SECURITY.md
├── .gitignore
├── docs/
│   ├── setup.md
│   └── architecture.md
└── workflows/
    ├── telegram-ai-personal-assistant.json      (main workflow)
    ├── email-management-sub-workflow.json
    ├── calendar-management-sub-workflow.json
    ├── research-agent-sub-workflow.json
    ├── contact-agent-sub-workflow.json
    └── x-posts-agent-sub-workflow.json
```

## Getting Started

1. Clone or download this repository.
2. Read [Security Policy](SECURITY.md) before sharing or importing workflow files.
3. Follow the instructions in the [Setup Guide](docs/setup.md); see [Architecture](docs/architecture.md) for how the workflows fit together.
4. Import the five sub-workflows into your n8n instance.
5. Import the main workflow.
6. Configure credentials, resources (Google Sheets IDs, data table, `YOUR_NAME` email signature), and sub-workflow references.
7. Test each workflow with non-production data before activating it.

## Important Notes

The files in [`workflows/`](workflows/) are sanitized exports, not a guarantee of a ready-to-run installation. Credentials, resource IDs, and workflow references may need to be configured for your own n8n instance.

Uploading workflow files to GitHub does not automatically update the workflows running in n8n.

## Security

Never commit API keys, passwords, OAuth tokens, private webhook URLs, or real customer data. Review every exported JSON file before making the repository public.

See [SECURITY.md](SECURITY.md) for additional guidance.

## Project Goal

The goal of this project is to demonstrate how a conversational AI assistant can coordinate multiple automation tasks through a modular n8n architecture, making everyday digital operations easier to manage from a single interface.
