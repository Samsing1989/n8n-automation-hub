# 15 — Instagram Leads

Listens for Instagram comment webhooks, detects buying-intent keywords (price, buy, order), saves hot leads to Google Sheets, and notifies the manager by email. All comments (leads and non-leads) are logged to a separate sheet.

## What it does

1. Receives Instagram comment webhook (via Meta Graph API)
2. Checks if comment contains buying-intent keywords
3. **Lead detected** → saves to Leads sheet → email notification to manager
4. **All comments** → logged to All Comments sheet regardless of keywords

## Time saved

Never miss a hot lead in Instagram comments. Zero manual monitoring needed.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Instagram Webhook | Webhook | Receives Meta Graph API events |
| Check Keywords | IF | Detects: price / buy / order / how much |
| Save Lead to Sheets | Google Sheets | Appends to Leads sheet |
| Notify Manager by Email | Gmail | Sends lead details to manager |
| Log All Comments | Google Sheets | Logs every comment regardless |

## Integrations

`n8n` · `Instagram (Meta Graph API)` · `Google Sheets` · `Gmail`

## Setup

1. Import `workflow.json` into n8n
2. Set up Google Sheets credential (OAuth2)
3. Set up Gmail credential (OAuth2)
4. Replace `YOUR_SPREADSHEET_ID` with your Google Sheet ID
5. Replace `YOUR_EMAIL` with your email address
6. Activate workflow and copy the webhook URL
7. In Meta for Developers → your app → Webhooks → subscribe to `comments` field on your Instagram Business account, paste the webhook URL

## Meta webhook verification

When subscribing, Meta will send a `GET` request to verify the webhook. You may need to add a verification node before the main flow (check n8n docs for webhook verification pattern).

## Google Sheets structure

**Leads sheet:**
```
Timestamp | Username | Comment | Type | Processed
```

**All Comments sheet:**
```
Timestamp | Username | Comment
```

## Keyword detection

Default keywords: `price`, `buy`, `order`, `how much`

To add more, duplicate conditions in the IF node with `OR` combinator.

## Screenshot

![workflow](screenshot.png)
