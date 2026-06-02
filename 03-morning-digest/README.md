# 03 — Daily Morning Digest

Every morning at 8:00 AM this workflow pulls active tasks from Google Sheets, builds a prioritized digest, and sends it to Telegram. Urgent tasks (due today) get a separate alert first.

## What it does

1. Triggers every day at 8:00 AM
2. Fetches all rows with status = "active" from Google Sheets
3. Builds a formatted digest with priority icons (🔴 high / 🟡 medium / 🟢 low)
4. Detects tasks with today's deadline → sends urgent alert first
5. Sends the full digest to Telegram

## Time saved

Replaces manual daily task review. Starts every morning with full context in under 1 second.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Schedule Trigger | Schedule | Fires at 8:00 AM daily |
| Get Rows | Google Sheets | Fetches active tasks |
| Build Digest | Code (JS) | Formats message + detects urgent |
| Has Urgent | IF | Checks if any task is due today |
| Send Urgent | Telegram | Sends urgent alert |
| Send Digest | Telegram | Sends full task list |

## Integrations

`n8n` · `Google Sheets` · `Telegram` · `JavaScript`

## Setup

1. Import `workflow.json` into n8n
2. Set up Google Sheets credential (OAuth2)
3. Set up Telegram Bot credential
4. Replace `YOUR_SPREADSHEET_ID` with your sheet ID
5. Replace `YOUR_CHAT_ID` with your Telegram chat ID
6. Activate the workflow

## Google Sheets structure

Your tasks sheet needs these columns:
```
task | priority | deadline | status
```

- `priority`: `high` / `medium` / `low`
- `deadline`: format `YYYY.MM.DD` (e.g. `2026.06.15`)
- `status`: `active` / `done`

## Screenshot

![workflow](screenshot.png)
