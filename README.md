# n8n Telegram Automations

A collection of n8n workflows that expose AI-powered automations through a Telegram bot interface. Each automation is self-contained and shares the same Telegram bot credential.

---

## Projects

### Telegram_Bot
A general-purpose AI assistant accessible via Telegram. Handles open-ended questions and tasks using an OpenAI language model. No external service integrations — pure conversational AI.

- **Trigger:** Telegram message
- **Model:** GPT (OpenAI)
- **Tools:** None
- **README:** `Telegram_Bot/README.md`

---

### Gmail_Automation
An AI agent that manages Gmail through Telegram commands. Supports reading and summarizing emails, sending emails, or both in a single request. Returns a structured execution report after each action.

- **Trigger:** Telegram message
- **Model:** GPT (OpenAI)
- **Tools:** Gmail (read, send)
- **README:** `Gmail_Automation/README.md`

---

### Calendar_Automation
A conversational meeting scheduling agent. Guides the user through gathering all required meeting details, checks for conflicts, creates Google Calendar events, and maintains per-user memory across multi-turn conversations.

- **Trigger:** Telegram message
- **Model:** GPT (OpenAI)
- **Tools:** Google Calendar (create event)
- **Memory:** Per-user conversation buffer
- **README:** `Calendar_Automation/README.md`

---

## Shared Infrastructure

| Component | Detail |
|---|---|
| Trigger interface | Telegram (same bot token across all workflows) |
| LLM provider | OpenAI (n8n free credits or bring your own key) |
| n8n credential name | `Batman` (Telegram), `n8n free OpenAI API credits` (OpenAI) |

---

## Credentials Required

| Service | Used By |
|---|---|
| Telegram Bot API | All three workflows |
| OpenAI API | All three workflows |
| Gmail OAuth2 | Gmail_Automation |
| Google Calendar OAuth2 | Calendar_Automation |

---

## Setup

1. Clone or download this repository.
2. Import each workflow JSON into your n8n instance.
3. Configure credentials for each service listed above.
4. Activate the workflows individually.
5. All three can run in parallel on the same Telegram bot — the agent's system prompt determines which workflow handles which type of request.

---

## Notes

- Each workflow is stateless by default except Calendar_Automation, which uses a memory buffer window keyed to the Telegram user ID.
- The workflows are independent and do not share data between each other at runtime.
- To consolidate into a single entry point, consider routing all messages through one AI agent with all tools attached and a unified system prompt.
