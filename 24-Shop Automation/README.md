# 27 — Business Dashboard

Runs every 15 minutes, fetches 4 data sources in parallel (HubSpot CRM leads, revenue API, support tickets, USD/UAH rate), checks critical thresholds, sends Telegram alert if anything is off, and logs every snapshot to Google Sheets.

## What it does

1. Triggers every 15 minutes
2. Fetches today's new leads from HubSpot CRM
3. Fetches today's revenue from your API
4. Fetches active support tickets count
5. Fetches live USD/UAH exchange rate
6. Checks 3 critical conditions (OR logic):
   - Leads = 0 (no new leads today)
   - Revenue > threshold (spike alert)
   - Tickets > 20 (support overload)
7. Critical → Telegram alert to manager
8. Normal → logs snapshot to Google Sheets dashboard

## Time saved

24/7 business monitoring without any manual checks. Get alerted only when metrics need attention.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Schedule Trigger | Schedule | Every 15 minutes |
| Fetch CRM Leads | HTTP Request | HubSpot contacts created today |
| Fetch Revenue Today | HTTP Request | Your revenue API endpoint |
| Fetch Active Tickets | HTTP Request | Your support system API |
| Fetch USD Rate | HTTP Request | open.er-api.com (free) |
| Check Critical Metrics | IF | 3 threshold conditions (OR) |
| Alert Critical Telegram | Telegram | Alert with all metrics |
| Update Dashboard Sheet | Google Sheets | Logs snapshot |

## Integrations

`n8n` · `HubSpot API` · `Custom Revenue API` · `Custom Tickets API` · `Exchange Rate API` · `Google Sheets` · `Telegram`

## Setup

1. Import `workflow.json` into n8n
2. Set up HubSpot credential (Header Auth with Bearer token)
3. Replace `YOUR_REVENUE_API_ENDPOINT` with your actual revenue API URL
4. Replace `YOUR_TICKETS_API_ENDPOINT` with your support system API URL
5. Set up Google Sheets credential (OAuth2)
6. Set up Telegram Bot credential
7. Replace `YOUR_SPREADSHEET_ID` with your dashboard sheet ID
8. Replace `YOUR_SPREADSHEET_URL` in Telegram node with your sheet URL
9. Replace `YOUR_CHAT_ID` with your Telegram chat ID
10. Adjust alert thresholds in the IF node to match your business
11. Activate the workflow

## Google Sheets structure

```
timestamp | leads | revenue | tickets | usd_rate | status
```

## Alert thresholds (customize in IF node)

| Metric | Default condition | Meaning |
|--------|------------------|---------|
| Leads | = 0 | No new leads today |
| Revenue | > 50 | Revenue spike |
| Tickets | > 20 | Support overload |

Change these to match your actual business scale.

## Revenue & Tickets API

Replace the mock URLs with your real APIs:
- Revenue: your CRM, Stripe, WooCommerce, or any sales system
- Tickets: Zendesk, Freshdesk, or custom helpdesk

## Screenshot

![workflow](screenshot.png)
