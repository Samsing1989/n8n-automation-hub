# 18 — Weekly Report

Every Friday at 17:00 reads Sales, Leads, and Orders data from Google Sheets, calculates key metrics, builds a styled HTML report, sends it by email, and posts a summary to Telegram.

## What it does

1. Triggers every Friday at 17:00
2. Reads 3 sheets: Sales / Leads / Orders
3. Calculates metrics via JavaScript
4. Builds a styled HTML email report
5. Sends full report to email via Gmail
6. Posts a compact summary to Telegram

## Metrics calculated

**Sales:** total revenue · deal count · average deal size

**Leads:** total count · qualified · closed · conversion rate %

**Orders:** total count · completed · total revenue · average order value

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Schedule Trigger | Schedule | Every Friday 17:00 |
| Read Sales Data | Google Sheets | Reads Sales sheet |
| Read Leads Data | Google Sheets | Reads Leads sheet |
| Read Orders Data | Google Sheets | Reads Orders sheet |
| Build Report Data | Code (JS) | Calculates all metrics |
| Prepare Report HTML | Set | Builds styled HTML |
| Send Report Email | Gmail | Sends full HTML report |
| Notify Report Sent | Telegram | Posts compact summary |

## Integrations

`n8n` · `Google Sheets` · `Gmail` · `Telegram` · `JavaScript`

## Setup

1. Import `workflow.json` into n8n
2. Set up Google Sheets credential (OAuth2)
3. Set up Gmail credential (OAuth2)
4. Set up Telegram Bot credential
5. Replace `YOUR_SPREADSHEET_ID` with your Google Sheet ID
6. Replace `YOUR_EMAIL` with your email address
7. Replace `YOUR_CHAT_ID` with your Telegram chat ID
8. Activate the workflow

## Google Sheets structure

**Sales sheet:**
```
Amount | Date | ...
```

**Leads sheet:**
```
Status | Date | ...
```
- `Status`: `Qualified` / `Closed` / other

**Orders sheet:**
```
Status | Amount | Date | ...
```
- `Status`: `Completed` / other

## Customize

- Add more sheets (expenses, refunds, etc.) by duplicating the Read node pattern
- Change trigger day/time in Schedule node
- Add Google Sheets logging of each report for historical tracking

## Screenshot

![workflow](screenshot.png)
