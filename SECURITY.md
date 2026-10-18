# Security Policy

## Purpose

This repository contains documentation and sanitized exports for the Telegram AI Personal Assistant built with n8n.

Workflow exports can contain sensitive configuration details. This policy explains how to protect credentials, personal information, and private infrastructure when working with this project.

## Never Commit Secrets

Do not upload the following information to this repository:

- API keys or access tokens.
- Passwords or OAuth refresh tokens.
- Private webhook URLs or authentication parameters.
- Session cookies or authorization headers.
- Personal email addresses or private contact details that should not be public.
- Real Telegram messages containing private information.
- Customer records, contact lists, or private calendar events.
- Unredacted workflow exports containing sensitive information.
- Private instance URLs or internal infrastructure details that should not be shared.
- Any other confidential business or personal information.

## Credential Management

Use the credential management features provided by n8n to store authentication information.

Do not hard-code credentials into:

- Code nodes.
- Workflow expressions.
- AI prompts.
- Documentation.
- Example configuration files.
- GitHub commits.

Where an integration requires authentication, configure the credential inside your own n8n instance.

## Sanitizing Workflow Exports

Before committing an n8n workflow export:

1. Export the workflow from n8n.
2. Remove or replace sensitive values.
3. Check credential references and authentication-related configuration.
4. Review webhook paths and private URLs.
5. Review pinned data, example messages, execution data, and other embedded sample information.
6. Replace private resource identifiers where necessary.
7. Search the JSON for email addresses, tokens, passwords, private hostnames, and customer information.
8. Inspect the final file manually before committing it.

Sanitization is a best-effort process. A file described as sanitized must still be reviewed before publication.

## Protect Production Data

Use test accounts and dummy data while developing and troubleshooting workflows.

Avoid testing destructive actions or live publishing operations against production resources until the workflow has been verified.

Grant integrations only the permissions needed for their intended tasks.

## Git and GitHub Safety

Review files before committing them.

The `.gitignore` file helps prevent accidental uploads of local secret files, but it does not replace manual review.

If a secret is committed accidentally:

1. Revoke or rotate the exposed credential immediately.
2. Check whether the credential was used or accessed.
3. Remove the exposed value from the repository.
4. Review Git history, because removing a secret from the latest version does not remove it from earlier commits.
5. Follow the relevant provider's incident-response recommendations.

## Reporting a Security Issue

Do not publish active credentials or sensitive technical details in a public GitHub issue.

Report security concerns privately to the repository owner or the person responsible for the affected n8n instance.

## Disclaimer

This document describes the project's security practices. It does not certify that a workflow export is free from secrets or that an imported workflow is secure by default. Every file and integration must be reviewed in its intended environment.
