# 13 — News Digest

Every day at 10:30 AM fetches top 10 news articles on your topic via NewsAPI, builds a styled HTML email digest, sends it to your inbox, posts headlines to Telegram, and logs to Google Sheets.

## What it does

1. Triggers every day at 10:30 AM
2. Fetches top 10 articles from NewsAPI by keyword
3. Builds a styled HTML email with titles, descriptions, and source links
4. Sends the digest to your email via Gmail
5. Posts article headlines + links to Telegram
6. Logs digest metadata to Google Sheets

## Time saved

Replaces manual news monitoring. Full briefing delivered automatically every morning.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Schedule Trigger | Schedule | Daily at 10:30 AM |
| Fetch News by Topic | HTTP Request | NewsAPI — top 10 articles |
| Build HTML Digest | Code (JS) | Builds HTML email + Telegram text |
| Send Digest Email | Gmail | Sends styled HTML email |
| Send to Telegram | Telegram | Posts headlines with links |
| Log Digest to Sheets | Google Sheets | Logs date, topic, sources |

## Integrations

`n8n` · `NewsAPI` · `Gmail` · `Telegram` · `Google Sheets` · `JavaScript`

## Setup

1. Import `workflow.json` into n8n
2. Get a free NewsAPI key at [newsapi.org](https://newsapi.org)
3. Add it as HTTP Header Auth credential: header name `X-Api-Key`, value = your key
4. Set up Gmail credential (OAuth2)
5. Set up Telegram Bot credential
6. Set up Google Sheets credential (OAuth2)
7. Replace `YOUR_SEARCH_TOPIC` in the HTTP Request node with your keyword (e.g. `AI automation`, `crypto`, `Ukraine`)
8. Replace `YOUR_EMAIL` with your email address
9. Replace `YOUR_CHAT_ID` with your Telegram chat ID
10. Replace `YOUR_SPREADSHEET_ID` with your Google Sheet ID
11. Activate the workflow

## Google Sheets structure

```
date | topic | articles_urls | source
```

## Customize

- Change language in query parameters (`en`, `uk`, `de`, etc.)
- Add multiple keywords with OR: `AI OR automation OR n8n`
- Change `pageSize` to get more or fewer articles
- Add a second fetch node for a second topic

## Screenshot

![workflow](screenshot.png)
