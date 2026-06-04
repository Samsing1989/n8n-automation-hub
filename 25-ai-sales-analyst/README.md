# 25 — AI Sales Analyst

Every Sunday reads last 7 days of sales data from Google Sheets, sends it to Groq AI for analysis, generates 3 specific recommendations, sends a styled HTML report by email, and logs to archive sheet.

## What it does

1. Triggers every Sunday
2. Reads sales data for the last 7 days from Google Sheets
3. Aggregates key metrics via JavaScript (revenue, deals, avg check, top product)
4. Sends aggregated data to Groq AI for analysis
5. Groq returns 3 specific actionable recommendations
6. Builds styled HTML email report with metrics + AI insights
7. Sends report by email
8. Archives analysis to log sheet

## Time saved

Replaces manual weekly sales review. Get AI-powered insights every Monday morning automatically.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Schedule Trigger | Schedule | Every Sunday |
| Get Sales Last 7 Days | Google Sheets | Reads weekly sales data |
| Aggregate Sales Data | Code (JS) | Calculates metrics |
| Groq AI Analysis | HTTP Request | AI generates recommendations |
| Build Report Data | Set | Combines metrics + AI output |
| Send Weekly Report Email | Gmail | Sends HTML report |
| Archive Analysis Log | Google Sheets | Logs to archive sheet |

## Integrations

`n8n` · `Google Sheets` · `Groq API` · `Gmail` · `JavaScript`

## Setup

1. Import `workflow.json` into n8n
2. Set up Google Sheets credential (OAuth2)
3. Get a free Groq API key at [console.groq.com](https://console.groq.com)
4. Add as HTTP Header Auth: `Authorization: Bearer YOUR_KEY`
5. Set up Gmail credential (OAuth2)
6. Replace `YOUR_SPREADSHEET_ID` with your sales sheet ID
7. Replace `YOUR_EMAIL` with your email address
8. Activate the workflow

## Google Sheets structure

**Sales sheet:**
```
date | product | amount | quantity | status
```

**Archive sheet:**
```
week | total_revenue | deals | avg_check | top_product | ai_recommendations
```

## Customize

- Change trigger day to any day of the week
- Edit the AI prompt to focus on different aspects (margins, churn, etc.)
- Add comparison with previous week for trend analysis

## Screenshot

![workflow](screenshot.png)
