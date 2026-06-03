# 14 — Onboarding Flow

Automated 3-email onboarding sequence triggered by a new client webhook. Sends a Welcome email on day 0, Instructions on day 1, and FAQ on day 4. Each step is logged to Google Sheets and confirmed via Telegram.

## What it does

1. Triggered by webhook (new client registered or paid)
2. **Day 0** — sends Welcome email → logs to Sheets → Telegram confirmation
3. Waits 1 day
4. **Day 1** — sends Instructions email → logs to Sheets → Telegram confirmation
5. Waits 3 more days
6. **Day 4** — sends FAQ email → logs to Sheets → Telegram confirmation

## Time saved

Eliminates manual follow-up emails. Every new client gets a consistent onboarding experience automatically.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| CRM Trigger | Webhook | Entry point — new client event |
| Email 1 - Welcome | Gmail | Day 0 welcome email |
| Telegram - After Email 1 | Telegram | Confirmation to manager |
| Sheets - Log Email 1 | Google Sheets | Logs send event |
| Wait 1 Day | Wait | 24-hour pause |
| Email 2 - Instructions | Gmail | Day 1 instructions email |
| Telegram - After Email 2 | Telegram | Confirmation to manager |
| Sheets - Log Email 2 | Google Sheets | Logs send event |
| Wait 3 Days | Wait | 72-hour pause |
| Email 3 - FAQ | Gmail | Day 4 FAQ email |
| Telegram - After Email 3 | Telegram | Confirmation to manager |
| Sheets - Log Email 3 | Google Sheets | Logs send event |

## Integrations

`n8n` · `Webhook` · `Gmail` · `Telegram` · `Google Sheets`

## Setup

1. Import `workflow.json` into n8n
2. Set up Gmail credential (OAuth2)
3. Set up Telegram Bot credential
4. Set up Google Sheets credential (OAuth2)
5. Replace `YOUR_SPREADSHEET_ID` with your Google Sheet ID
6. Replace `YOUR_CHAT_ID` with your Telegram chat ID
7. Replace `YOUR_EMAIL` in Send nodes with client email expression
8. Edit email content in each Gmail node to match your onboarding copy
9. Activate and copy the webhook URL to your CRM or payment system

## Expected webhook payload

```json
{
  "name": "John Doe",
  "email": "john@example.com"
}
```

## Google Sheets structure

```
timestamp | client_name | email | step | status
```

## Important note on Wait nodes

n8n's Wait node requires the workflow execution to stay active. Make sure your n8n instance is running continuously (self-hosted or cloud). If the execution is interrupted, the wait will not resume automatically.

## Screenshot

![workflow](screenshot.png)
