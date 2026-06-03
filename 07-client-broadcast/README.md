# 07 — Client Broadcast

Every Monday at 10:00 AM sends a personalized Telegram message with an inline CTA button to all active clients from Google Sheets. Logs every sent message to a separate Broadcast_Log sheet.

## What it does

1. Triggers every Monday at 10:00 AM
2. Reads all clients from Google Sheets
3. Filters only clients with status = `active`
4. Waits 2 seconds between messages (anti-spam)
5. Sends personalized message with inline keyboard buttons
6. Logs result (client name, chat ID, message ID, timestamp) to Broadcast_Log sheet

## Time saved

Replaces manual weekly outreach. One workflow broadcasts to unlimited clients automatically.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Weekly Broadcast Trigger | Schedule | Every Monday 10:00 AM |
| Get Active Clients | Google Sheets | Reads full clients list |
| Filter Active Only | IF | Keeps only status = active |
| Anti-Spam Wait | Wait | 2 sec pause between sends |
| Send Personalized Message | Telegram | Sends message with inline buttons |
| Log Sent Result | Set | Builds log object |
| Log to Broadcast_Log | Google Sheets | Appends to log sheet |

## Integrations

`n8n` · `Google Sheets` · `Telegram`

## Setup

1. Import `workflow.json` into n8n
2. Set up Google Sheets credential (OAuth2)
3. Set up Telegram Bot credential
4. Replace `YOUR_SPREADSHEET_ID` with your Google Sheet ID
5. Replace `YOUR_USERNAME` in button URLs with your Telegram username
6. Edit the message text in the Telegram node to match your offer
7. Activate the workflow

## Google Sheets structure

**Clients sheet:**
```
name | chat_id | status
```
- `status`: `active` / `inactive`
- `chat_id`: Telegram user chat ID (users must have started your bot first)

**Broadcast_Log sheet:**
```
timestamp | client_name | chat_id | status | message_id
```

## Important

> Clients must have previously started your Telegram bot — otherwise the message send will fail. You can collect chat IDs via a `/start` command bot (see WF-02).

## Screenshot

![workflow](screenshot.png)
