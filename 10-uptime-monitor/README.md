# 10 — Uptime Monitor

Checks your website availability every 5 minutes. Sends a Telegram alert when the site goes down, and another when it recovers. Logs every check to Google Sheets.

## What it does

1. Triggers every 5 minutes
2. Makes HTTP GET request to your site
3. Reads last known status from Google Sheets
4. Compares current vs previous status
5. **Site down** → sends 🚨 DOWN alert to Telegram
6. **Site recovered** → sends ✅ RECOVERY alert to Telegram
7. Logs every check (URL, status, code, timestamp) to Sheets

## Time saved

24/7 monitoring without any paid service. Know about downtime before your clients do.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Schedule Trigger | Schedule | Every 5 minutes |
| Check Site | HTTP Request | GET request to your URL |
| Get Last Status | Google Sheets | Reads previous status row |
| Compare Status | Code (JS) | Detects down / recovered / stable |
| Is Site Down | IF | Routes by current status |
| Just Recovered | IF | Detects recovery event |
| Notify Site Down | Telegram | 🚨 Down alert |
| Notify Site Recovered | Telegram | ✅ Recovery alert |
| Log Status | Google Sheets | Appends check result |

## Integrations

`n8n` · `HTTP Request` · `Google Sheets` · `Telegram` · `JavaScript`

## Setup

1. Import `workflow.json` into n8n
2. Set up Google Sheets credential (OAuth2)
3. Set up Telegram Bot credential
4. Replace `YOUR_SITE_URL.com` in the HTTP Request node and Code node with your actual domain
5. Replace `YOUR_SPREADSHEET_ID` with your Google Sheet ID
6. Replace `YOUR_CHAT_ID` with your Telegram chat ID
7. Activate the workflow

## Google Sheets structure

```
timestamp | URL | status | code
```

- `status`: `ok` / `OFFLINE`
- `code`: HTTP status code (200, 500, 0 for timeout, etc.)

## How alerts work

- Alert fires only on **status change** — not every check
- If site was `ok` and becomes `OFFLINE` → DOWN alert
- If site was `OFFLINE` and becomes `ok` → RECOVERY alert
- If status unchanged → only logs to Sheets, no Telegram message

## Customize

- Monitor multiple sites by duplicating the workflow
- Add email notification alongside Telegram (Gmail node)
- Lower interval to 1 minute for critical services

## Screenshot

![workflow](screenshot.png)
