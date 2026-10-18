# Telegram AI Personal Assistant

An AI-powered personal assistant built with **n8n, Telegram, and AI integrations**. The project allows users to interact with an assistant through Telegram and delegate tasks to specialized automation workflows.

The system uses a main workflow to coordinate user requests and five sub-workflows to organize different responsibilities.

## Project Overview

The Telegram AI Personal Assistant is designed to simplify everyday tasks by connecting a conversational interface to automation workflows.

Instead of manually switching between multiple applications, a user can send a request through Telegram. The main workflow processes the request and routes it to the appropriate sub-workflow.

### Key Features

- **Telegram Interface:** Provides the primary user interaction channel for text requests and supported voice-message handling.
- **AI-Powered Coordination:** The main assistant interprets requests using the AI components configured in the workflow.
- **Email Management:** Handles email-related requests through the configured email integration.
- **Calendar Management:** Handles calendar-related tasks through the configured calendar integration.
- **Research:** Supports research requests using the sources configured in the workflow.
- **Contact Management:** Uses Google Sheets for configured contact-related operations.
- **X (Twitter) Posts:** Supports X-post-related operations using the configured Google Sheets integration.
- **Modular Architecture:** Separates responsibilities into sub-workflows to make maintenance and troubleshooting easier.

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

1. The user sends a request through Telegram.
2. The main workflow receives and processes the request.
3. The assistant determines which task workflow should handle the request.
4. The relevant sub-workflow executes its configured operations.
5. The result is returned to the main workflow.
6. The assistant sends a response through Telegram.

Voice-message functionality depends on the transcription and message-processing nodes configured in the current workflow version.

## Technology Stack

- **n8n:** Workflow automation and orchestration.
- **Telegram:** User-facing messaging interface.
- **AI model integration:** Request interpretation and assistant reasoning.
- **Email integration:** Email-related operations.
- **Calendar integration:** Calendar-related operations.
- **Google Sheets:** Configured contact and X-post data operations.
- **Research sources:** The information sources configured in the Research workflow.

The exact providers and capabilities depend on the credentials and nodes configured in the imported workflows.

## Repository Structure

```text
Telegram-AI-Personal-Assistant/
├── README.md
├── SECURITY.md
├── .gitignore
├── docs/
│   ├── setup.md
│   └── architecture.md
├── Telegram-AI-Personal-Assistant-Json.json      (main workflow)
├── Email-Management-Sub-Workflow-Json.json
├── Calendar_Management_Agent_Json.json
├── Research_Agent_Json.json
├── Contact_Agent_Sub_workflow_Json.json
└── X_Posts_Agent_Sub_Workflow_Json.json
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

The workflow JSON files are exports, not a guarantee of a ready-to-run installation. Credentials, resource IDs, and workflow references may need to be configured for your own n8n instance.

Uploading workflow files to GitHub does not automatically update the workflows running in n8n.

## Security

Never commit API keys, passwords, OAuth tokens, private webhook URLs, or real customer data. Review every exported JSON file before making the repository public.

See [SECURITY.md](SECURITY.md) for additional guidance.

## Project Goal

The goal of this project is to demonstrate how a conversational AI assistant can coordinate multiple automation tasks through a modular n8n architecture, making everyday digital operations easier to manage from a single interface.
