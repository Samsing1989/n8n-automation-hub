# 11 — CRM Auto-fill

New lead from a web form automatically creates a HubSpot contact, saves to Google Sheets, and notifies the manager via Telegram with a direct link to the HubSpot record.

## What it does

1. Receives POST request from any web form
2. Creates a new contact in HubSpot CRM via API
3. Saves lead data + HubSpot ID and URL to Google Sheets
4. Sends Telegram notification with clickable HubSpot link
5. Returns HTTP 200 to the form

## Time saved

Eliminates manual CRM data entry. Every lead is in HubSpot + Sheets + Telegram in under 3 seconds.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Receive Lead Form | Webhook | Entry point — receives form POST |
| Create HubSpot Contact | HTTP Request | Creates contact via HubSpot API |
| Prepare Sheet Data | Set | Extracts contact ID and URL |
| Save Lead to Sheets | Google Sheets | Logs full lead to CRM sheet |
| Notify Manager Telegram | Telegram | Alert with HubSpot link |
| Confirm Lead Received | Respond to Webhook | Returns HTTP 200 |

## Integrations

`n8n` · `Webhook` · `HubSpot API` · `Google Sheets` · `Telegram`

## Setup

1. Import `workflow.json` into n8n
2. Create a HubSpot Private App and get the access token
3. Set up HubSpot App Token credential in n8n
4. Set up Google Sheets credential (OAuth2)
5. Set up Telegram Bot credential
6. Replace `YOUR_SPREADSHEET_ID` with your Google Sheet ID
7. Replace `YOUR_CHAT_ID` with your Telegram chat ID
8. Activate and copy the webhook URL to your form

## Expected POST body

```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "+1234567890",
  "company": "Acme Inc"
}
```

## Google Sheets structure

```
Date | Name | Email | Phone | Company | HubSpot ID | HubSpot URL
```

## Screenshot

![workflow](screenshot.png)
