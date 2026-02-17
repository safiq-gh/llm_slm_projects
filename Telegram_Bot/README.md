# Telegram AI Bot — n8n Workflow

A simple n8n automation that connects a Telegram bot to an OpenAI language model. Users send a message via Telegram and receive an AI-generated response.

---

## Overview

**Trigger:** Incoming Telegram message  
**Processing:** OpenAI GPT model via n8n AI Agent  
**Output:** Reply sent back to the same Telegram chat

---

## Workflow Nodes

| Node | Type | Role |
|---|---|---|
| Telegram Trigger | Webhook | Receives incoming messages |
| AI Agent | LangChain Agent | Processes input and generates a response |
| OpenAI Chat Model | LLM | Powers the agent (gpt-5-mini) |
| Send a text message | Telegram | Sends the response back to the user |

---

## Data Flow

```
Telegram Trigger → AI Agent → Send a text message
                       ↑
               OpenAI Chat Model
```

---

## Configuration

### Credentials Required

- **Telegram API** — Bot token (used by both the trigger and the send node)
- **OpenAI API** — Used via n8n's built-in free credits or your own key

### Agent System Prompt

The AI Agent is configured with a detailed system prompt that enforces:

- Accuracy over verbosity
- Direct, structured responses
- Professional neutrality
- Explicit uncertainty when information is insufficient

The full prompt is embedded in the AI Agent node under `options.systemMessage`.

---

## Setup

1. Import the workflow JSON into your n8n instance.
2. Connect your Telegram bot credentials under the `Batman` credential slot, or create new ones.
3. Add your OpenAI API credentials.
4. Activate the workflow.
5. Send a message to your Telegram bot to test.

---

## Notes

- The workflow currently has no memory — each message is treated as a standalone prompt with no conversation history.
- The model is set to `gpt-5-mini`. Update this in the OpenAI Chat Model node if needed.
- The Telegram `chatId` is extracted dynamically from the incoming message, so the reply always goes to the correct chat.