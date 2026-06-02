# 05 — Lead Validator

An advanced lead capture webhook that validates email format, saves qualified leads to Google Sheets, notifies the manager via Telegram, and returns proper HTTP responses to the form.

## What it does

1. Receives POST request from any web form
2. Validates that email contains `@`
3. **Valid lead** → saves to Google Sheets → Telegram notification → HTTP 200 response
4. **Invalid email** → HTTP 400 response with error message

## Difference from WF-01

WF-05 adds:
- Email format validation (checks for `@`)
- Proper HTTP response codes (200 / 400) sent back to the form
- Phone and Source fields tracked
- Cleaner manager notification format

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Webhook | Webhook | Receives form POST with responseNode mode |
| Validate Lead | IF | Checks email contains @ |
| Save Lead | Google Sheets | Appends to CRM sheet |
| Notify Manager | Telegram | Sends lead summary |
| Success Response | Respond to Webhook | Returns HTTP 200 |
| Rejected Response | Respond to Webhook | Returns HTTP 400 |

## Integrations

`n8n` · `Webhook` · `Google Sheets` · `Telegram`

## Setup

1. Import `workflow.json` into n8n
2. Set up Google Sheets credential (OAuth2)
3. Set up Telegram Bot credential
4. Replace `YOUR_SPREADSHEET_ID` with your Google Sheet ID
5. Replace `YOUR_CHAT_ID` with your Telegram chat ID
6. Activate and copy the webhook URL to your form

## Expected POST body

```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "+380501234567",
  "source": "website"
}
```

## Google Sheets structure

```
Date | Name | Email | Phone | Source
```

## HTTP Responses

**Success (200):**
```json
{ "status": "ok", "message": "Thank you! We will contact you shortly." }
```

**Error (400):**
```json
{ "error": "Invalid email", "message": "Please enter a valid email address." }
```

## Screenshot

![workflow](screenshot.png)
