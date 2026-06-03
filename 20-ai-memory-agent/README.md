# 20 — AI Memory Agent

A Telegram bot powered by Groq AI that remembers conversation history. Stores each message and reply in Google Sheets, loads context on every request, and gives contextually aware answers — like a real assistant.

## What it does

1. Receives a Telegram message
2. Extracts user ID, name, and message text
3. Checks if conversation history exists in Sheets
4. Loads last N messages as context
5. Formats history into Groq-compatible messages array
6. Sends full context + new message to Groq AI
7. Saves user message and AI reply to Sheets
8. Sends AI reply to Telegram

## Why this matters

Standard AI bots forget everything between messages. This workflow gives the AI persistent memory — it can refer to earlier parts of the conversation, remember user preferences, and give consistent answers.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Receive User Message | Telegram Trigger | Listens for messages |
| Extract Message Data | Code (JS) | Extracts user ID, name, text |
| Check Memory Exists | IF | Checks if history rows exist |
| Clear Old Memory | Google Sheets | Removes old rows (keeps last N) |
| Load Conversation History | Google Sheets | Reads recent messages |
| Format History for AI | Code (JS) | Builds messages array with roles |
| Ask Groq with Memory | HTTP Request | Calls Groq with full context |
| Parse AI Response | Code (JS) | Extracts answer text |
| Save User Message | Google Sheets | Logs user turn |
| Save AI Response | Google Sheets | Logs AI turn |
| Send Reply to User | Telegram | Delivers answer |
| Delete Typing Indicator | Telegram | Optional UX cleanup |

## Integrations

`n8n` · `Telegram` · `Groq API` · `Google Sheets` · `JavaScript`

## Setup

1. Import `workflow.json` into n8n
2. Get a free Groq API key at [console.groq.com](https://console.groq.com)
3. Add as HTTP Header Auth: `Authorization: Bearer YOUR_KEY`
4. Set up Telegram Bot credential
5. Set up Google Sheets credential (OAuth2)
6. Replace `YOUR_SPREADSHEET_ID` with your memory sheet ID
7. Replace `YOUR_CHAT_ID` if needed for admin notifications
8. Edit the system prompt in the Groq node to define your assistant's persona
9. Activate the workflow

## Google Sheets structure

```
timestamp | user_id | user_name | role | message
```
- `role`: `user` / `assistant`

## Memory management

The workflow keeps the last N messages per user (configurable in the Clear Old Memory node). This prevents the context window from growing too large while maintaining meaningful conversation history.

## Customize

- Swap Groq for OpenAI by changing the HTTP Request URL and auth header
- Add a `/reset` command to clear memory for a user
- Add multiple personas by checking the first message for a keyword

## Screenshot

![workflow](screenshot.png)
