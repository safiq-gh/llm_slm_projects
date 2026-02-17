# Meeting Scheduling Agent — n8n Workflow

An n8n automation that acts as a conversational meeting scheduling assistant via Telegram. The agent gathers meeting details through natural dialogue, checks for conflicts, creates Google Calendar events, and maintains context across the conversation.

---

## Overview

**Trigger:** Incoming Telegram message  
**Processing:** OpenAI GPT model via n8n AI Agent with Google Calendar tool  
**Memory:** Per-user conversation buffer for multi-turn scheduling flows  
**Output:** Agent reply sent back to the same Telegram chat

---

## Workflow Nodes

| Node | Type | Role |
|---|---|---|
| Telegram Trigger | Telegram Trigger | Receives incoming messages |
| AI Agent | LangChain Agent | Drives the scheduling conversation |
| OpenAI Chat Model | LLM (gpt-5-mini) | Powers the agent |
| Simple Memory | Memory Buffer Window | Maintains per-user conversation context |
| Create an event in Google Calendar | Google Calendar Tool | Creates the calendar event when confirmed |
| Send a text message | Telegram | Sends the agent's reply to the user |

---

## Data Flow

```
Telegram Trigger → AI Agent → Send a text message
                      ↑ ↓
               OpenAI Chat Model
               Simple Memory
               Google Calendar Tool
```

---

## Agent Behavior

The agent follows a structured conversational protocol before creating any event:

**Required fields before scheduling:**
- Meeting title / purpose
- Date and time (with timezone — always stated explicitly)
- Duration
- Participants
- Format (virtual or in-person)

**Conflict handling:** If a requested time is unavailable, the agent proposes 2–3 alternative slots rather than failing silently.

**Confirmation step:** Before creating the event, the agent restates all final details for the user to confirm.

**Tone:** Concise, professional, and direct. No filler language.

---

## Configuration

### Credentials Required

| Credential | Used By |
|---|---|
| Telegram API (`Batman`) | Telegram Trigger + Send a text message |
| OpenAI API | OpenAI Chat Model |
| Google Calendar OAuth2 | Create an event in Google Calendar |

### Calendar

Events are created on the calendar associated with the authenticated Google Calendar account. Update the calendar selection in the Google Calendar Tool node to change this.

### Memory

Conversation memory is keyed to the Telegram user ID (`message.from.id`), so each user maintains their own independent session. The default context window is 10 messages — adjust in the Simple Memory node if longer conversations are needed.

---

## Setup

1. Import the workflow JSON into your n8n instance.
2. Connect your Telegram bot credentials.
3. Add your OpenAI API credentials.
4. Authorize your Google Calendar account via OAuth2.
5. Update the target calendar in the Google Calendar Tool node if needed.
6. Activate the workflow.
7. Send a message to your bot to begin (e.g., "Schedule a call with John tomorrow at 3pm CET").

---

## Notes

- The agent does not yet send confirmation emails to attendees — add a Gmail tool node if this is needed.
- There is no explicit conflict-checking tool connected; conflict detection relies on the agent's reasoning from memory and conversation context. For strict conflict checking, add a Google Calendar "Get Events" tool node.
- The model is set to `gpt-5-mini`. For complex multi-turn scheduling flows, upgrading to `gpt-4o` will improve reliability.