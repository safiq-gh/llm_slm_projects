# Telegram Gmail Assistant — n8n Workflow

An n8n automation that lets you manage Gmail through a Telegram bot. Send a natural language command via Telegram, and the AI agent will read or send emails on your behalf, then report back with a structured summary.

---

## Overview

**Trigger:** Incoming Telegram message  
**Processing:** OpenAI GPT model via n8n AI Agent with Gmail tools  
**Output:** Execution report sent back to the same Telegram chat

---

## Workflow Nodes

| Node | Type | Role |
|---|---|---|
| TeleTrigg | Telegram Trigger | Receives incoming messages |
| AI Agent | LangChain Agent | Interprets the request and orchestrates tools |
| GPT | OpenAI Chat Model (gpt-5-mini) | Powers the agent |
| Get many messages in Gmail | Gmail Tool | Fetches emails from inbox |
| Send a message in Gmail | Gmail Tool | Sends emails via Gmail |
| Send a text message | Telegram | Returns the execution report to the user |

---

## Data Flow

```
TeleTrigg → AI Agent → Send a text message
               ↑ ↓
              GPT (LLM)
               ↑ ↓
      Gmail Tools (read / send)
```

---

## Supported Actions

The agent handles three task types based on the user's message:

| Command Intent | Action |
|---|---|
| Summarize an email | Fetches and summarizes inbox messages |
| Send an email | Composes and sends an email via Gmail |
| Summarize and send | Summarizes an email and sends the summary |

After completing the task, the agent replies with a structured execution report — not the raw email content.

### Report Format

```
Task Performed:
Email Summary / Email Sent / Email Summary & Sent

Details:
- [What was done, recipient if applicable]

Status:
Success / Failed
```

---

## Configuration

### Credentials Required

- **Telegram API** — Bot token (credential name: `Batman`)
- **OpenAI API** — Used via n8n's built-in free credits or your own key
- **Gmail OAuth2** — Connected Gmail account authorized for read and send access

### Send-to Address

The send node has a hardcoded recipient address. Update this in the `Send a message in Gmail` node if needed, or modify the agent prompt to accept dynamic recipients from the user's message.

---

## Setup

1. Import the workflow JSON into your n8n instance.
2. Connect your Telegram bot credentials.
3. Add your OpenAI API credentials.
4. Authorize your Gmail account via OAuth2.
5. Update the hardcoded recipient email in the send node if required.
6. Activate the workflow.
7. Send a command to your Telegram bot to test (e.g., "Summarize my last 3 emails").

---

## Notes

- The agent has no conversation memory — each Telegram message is a standalone request.
- Email content is not included in the Telegram reply unless explicitly requested by the user.
- The number of emails fetched is determined dynamically by the agent based on the user's request.
- If an error occurs, the agent sets `Status: Failed` and describes the issue briefly in the report.