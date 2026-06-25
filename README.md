# AI-Assisted Ticket Processing Workflow

This project demonstrates an AI-assisted workflow for processing customer support tickets. It automatically classifies incoming requests using an LLM, stores ticket information in a PostgreSQL database, supports human approval via Telegram, and routes tickets based on predefined business rules.

## How it works

- **Webhook Trigger:** Incoming customer requests are received through a webhook and transformed into a structured ticket.

- **Duplicate Detection:** PostgreSQL checks whether the incoming ticket has already been processed using the ticket ID. Duplicate requests are ignored to avoid unnecessary processing and repeated AI calls.

- **AI-Based Ticket Classification:** The ticket content is analyzed by **Llama 3.3 (via the Groq API)**. The model determines the ticket category, priority, and returns a structured JSON response.

- **Error Handling:** Invalid or unexpected AI responses are handled using a dedicated Try/Catch mechanism to keep the workflow stable.

- **State Management:** Ticket information is stored in PostgreSQL with an initial status of **pending_approval**, allowing the workflow to track the current processing state.

- **Human Approval:** The workflow sends the ticket summary to Telegram using n8n's **Send and Wait for Response** node. A user can approve or reject the ticket directly from Telegram.

- **Audit Logging:** After a decision is made, the workflow stores the responsible user and the decision timestamp in PostgreSQL for traceability.

- **Business Routing:** Based on the approval status and ticket category, the workflow routes the ticket to different simulated target systems (e.g. Jira Service Management, SAP ERP, Salesforce) or a manual review queue.

## Tech Stack

- **Workflow Automation:** n8n
- **Programming Language:** JavaScript
- **LLM:** Llama 3.3 (70B) via Groq API
- **Database:** PostgreSQL
- **Notifications:** Telegram Bot API

## Monitoring

This workflow is monitored by a shared n8n **Error Trigger** workflow. If an unexpected error occurs during execution, a Telegram alert is automatically sent containing the error message and debugging information.

## Repository Contents

- **ticket_workflow.json** – Main workflow including ticket classification, approval process, database integration, and routing logic.
