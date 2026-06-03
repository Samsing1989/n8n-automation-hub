# 19 — Error Assistant

A global error handler for all n8n workflows. Catches any workflow failure, classifies the error type, logs to Google Sheets, sends a Telegram alert, and emails a detailed report for critical errors.

## What it does

1. Triggered automatically when any linked workflow fails
2. Classifies error type via regex Switch: AUTH / TIMEOUT / RATE_LIMIT / NETWORK / INFO
3. Logs full error details to Google Sheets
4. Sends Telegram alert with workflow name, node, error message, and execution link
5. For critical errors → also sends a detailed HTML email

## Error classification

| Type | Trigger keywords | Severity |
|------|-----------------|----------|
| AUTH_ERROR | 401, 403, unauthorized, forbidden | 🔴 Critical |
| TIMEOUT | timeout, ECONNRESET, ETIMEDOUT | 🟠 Serious |
| RATE_LIMIT | rate limit, 429, too many requests | 🟡 Warning |
| NETWORK | ENOTFOUND, network, connection refused | 🟡 Warning |
| INFO | anything else | 🔵 Info |

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Error Trigger | Error Trigger | Catches failures from linked workflows |
| Classify Error | Switch | Routes by error type regex |
| Set Error Type | Set | Assigns emoji + label |
| Log to Sheets | Google Sheets | Appends full error row |
| Telegram Alert | Telegram | Instant notification |
| Is Critical | IF | Checks if critical keywords present |
| Critical Alert Email | Gmail | Detailed HTML email for critical errors |

## Integrations

`n8n` · `Google Sheets` · `Telegram` · `Gmail`

## Setup

1. Import `workflow.json` into n8n
2. Set up Google Sheets credential (OAuth2)
3. Set up Telegram Bot credential
4. Set up Gmail credential (OAuth2)
5. Replace `YOUR_SPREADSHEET_ID` with your error log sheet ID
6. Replace `YOUR_CHAT_ID` with your Telegram chat ID
7. Replace `YOUR_EMAIL` with your email
8. **Activate this workflow**
9. In each other workflow → Settings → Error Workflow → select this workflow

## Google Sheets structure

```
Timestamp | Workflow Name | Node Name | Error Type | Error Message | Execution ID | Execution URL
```

## Important

> This workflow must be **active** at all times. It is a system workflow — set it and forget it. Link it to all your production workflows via Settings → Error Workflow.

## Screenshot

![workflow](screenshot.png)
