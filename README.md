# 01 — Lead Capture with Validation

Webhook receives a contact form submission, validates the email field, saves the lead to Google Sheets, and notifies the owner via Telegram.

## What it does

1. Receives POST request from a contact form
2. Extracts name, email, and message fields
3. Checks if email is not empty
4. If valid → saves to Google Sheets + Telegram notification to owner
5. If invalid → sends alert about missing email

## Time saved

~2–3 hours/week of manual lead tracking

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Receive Lead Form | Webhook | Entry point — receives form data |
| Extract Fields | Set | Pulls name, email, message |
| Validate Email | IF | Checks email is not empty |
| Save Lead to Sheets | Google Sheets | Appends lead to CRM sheet |
| Notify Owner | Telegram | Sends lead details to owner |
| Invalid Email Response | Telegram | Alerts about missing email |

## Integrations

`n8n` · `Webhook` · `Google Sheets` · `Telegram`

## Setup

1. Import `workflow.json` into n8n
2. Set up Google Sheets credential (OAuth2)
3. Set up Telegram Bot credential
4. Replace `YOUR_SPREADSHEET_ID` with your Google Sheet ID
5. Replace `YOUR_CHAT_ID` with your Telegram chat ID
6. Activate the workflow and copy the webhook URL to your form

## Google Sheets structure

Your sheet should have these columns:
```
name | email | message | timestamp
```

## Screenshot

![workflow](screenshot.png)
