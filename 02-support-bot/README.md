# 02 — Telegram Support Bot with Categories

A Telegram bot that routes user commands (/start, /help, /about) to the right response and logs every interaction to Google Sheets.

## What it does

1. Listens for incoming Telegram messages
2. Routes by command using a Switch node
3. Sends the correct response for each command
4. Falls back to a default message for unknown input
5. Logs every interaction (user ID, name, command, timestamp) to Google Sheets

## Time saved

Eliminates manual support replies for common questions. Runs 24/7 without intervention.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Telegram Trigger | Trigger | Listens for incoming messages |
| Route by Command | Switch | Routes /start, /help, /about |
| /start | Telegram | Personalized greeting |
| /help | Telegram | Assistant intro message |
| /about | Telegram | Lists available commands |
| other | Telegram | Fallback for unknown commands |
| Log Message to Sheets | Google Sheets | Logs all interactions |

## Integrations

`n8n` · `Telegram` · `Google Sheets`

## Setup

1. Import `workflow.json` into n8n
2. Create a Telegram bot via [@BotFather](https://t.me/BotFather) and get the token
3. Set up Telegram Bot credential in n8n
4. Set up Google Sheets credential (OAuth2)
5. Replace `YOUR_SPREADSHEET_ID` with your Google Sheet ID
6. Activate the workflow

## Google Sheets structure

```
Timestamp | User ID | Name | Command
```

## Customization

- Edit the text in each Telegram node to match your bot's personality
- Add more commands by duplicating Switch rules and Telegram nodes
- Add an AI node (e.g. OpenAI) to the fallback branch for smart replies

## Screenshot

![workflow](screenshot.png)
