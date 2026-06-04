# 23 — Auto Invoicing

When a Trello card is moved to "Done", this workflow automatically generates an invoice via Zoho Invoice API, downloads the PDF, emails it to the client, logs to Google Sheets, and notifies you via Telegram. Duplicate protection prevents double invoicing.

## What it does

1. Receives Trello webhook when a card is moved
2. Parses card description to extract: CLIENT email, PROJECT name, AMOUNT, CURRENCY
3. Checks Google Sheets for duplicate invoice (same card ID)
4. If not a duplicate → creates invoice via Zoho Invoice API
5. Downloads invoice as PDF
6. Sends PDF to client by email
7. Logs invoice to Google Sheets
8. Sends Telegram notification

## Time saved

Eliminates manual invoice creation and sending. Every completed project triggers invoicing automatically.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Webhook | Webhook | Receives Trello card move event |
| Parse Task Data | Set | Extracts fields from card description |
| Check Duplicate Invoice | Google Sheets | Looks up card ID |
| Check Task Completed | IF | Skips if already invoiced |
| Generate Invoice API | HTTP Request | Creates invoice in Zoho |
| Download Invoice PDF | HTTP Request | Downloads PDF from Zoho |
| Send Invoice to Client | Gmail | Sends email with PDF |
| Log Invoice to Sheets | Google Sheets | Appends invoice record |
| Notify Invoice Sent | Telegram | Confirmation to manager |

## Integrations

`n8n` · `Trello Webhook` · `Zoho Invoice API` · `Gmail` · `Google Sheets` · `Telegram`

## Setup

1. Import `workflow.json` into n8n
2. Set up Zoho Invoice API credential (OAuth2 or API key as Header Auth)
3. Set up Gmail credential (OAuth2)
4. Set up Google Sheets credential (OAuth2)
5. Set up Telegram Bot credential
6. Replace `YOUR_SPREADSHEET_ID` with your invoices sheet ID
7. Replace `YOUR_CHAT_ID` with your Telegram chat ID
8. Activate workflow and copy the webhook URL
9. In Trello → Power-Ups → Webhooks → paste URL, set trigger to card update

## Trello card description format

The workflow parses the card description using regex. Format it exactly like this:

```
CLIENT: client@email.com
PROJECT: Website Redesign
AMOUNT: 1500
CURRENCY: USD
```

## Google Sheets structure

```
card_id | invoice_number | client_email | amount | currency | date_created | status
```

## Duplicate protection

Before generating, the workflow checks if a row with the same `card_id` already exists. If found, it skips — preventing double invoicing if Trello fires the webhook multiple times.

## Screenshot

![workflow](screenshot.png)
