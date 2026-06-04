# 26 — QA Feedback Bot

After a delivery, automatically sends a feedback request to the client, waits for their rating, logs the score to Google Sheets, and escalates low scores to the manager via Telegram and email.

## What it does

1. Triggered by webhook (project delivery event)
2. Reads client data from Google Sheets
3. Sends feedback request email to client
4. Logs request as "pending" in Sheets
5. Client submits rating via a form/link
6. Score ≥ 4 → updates Sheets status to "positive", no escalation
7. Score < 4 → updates Sheets + Telegram alert to manager + email escalation

## Time saved

Automates the entire post-delivery feedback cycle. Never miss a dissatisfied client — get notified instantly when something goes wrong.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Webhook | Webhook | Receives delivery completion event |
| IF | IF | Routes by rating score |
| Get row(s) in sheet | Google Sheets | Reads client data |
| Send a message | Gmail | Sends feedback request email |
| Update row in sheet | Google Sheets | Updates status on positive score |
| Update row in sheet1 | Google Sheets | Updates status on low score |
| Send a text message | Telegram | Escalation alert to manager |

## Integrations

`n8n` · `Webhook` · `Google Sheets` · `Gmail` · `Telegram`

## Setup

1. Import `workflow.json` into n8n
2. Set up Google Sheets credential (OAuth2)
3. Set up Gmail credential (OAuth2)
4. Set up Telegram Bot credential
5. Replace `YOUR_SPREADSHEET_ID` with your feedback sheet ID
6. Replace `YOUR_CHAT_ID` with your manager's Telegram chat ID
7. Replace `YOUR_EMAIL` with your email
8. Activate and copy webhook URL to your delivery system

## Google Sheets structure

```
project_id | client_email | client_name | delivery_date | score | status
```
- `status`: `pending` / `positive` / `escalated`

## Escalation threshold

Default: score < 4 triggers escalation. Change the IF node condition to adjust.

## Screenshot

![workflow](screenshot.png)
