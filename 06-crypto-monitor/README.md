# 06 — Crypto Monitor

Checks BTC price every hour via CoinGecko API, compares it to the previous value in Google Sheets, and sends a Telegram alert if the change exceeds 3%.

## What it does

1. Triggers every hour
2. Fetches current BTC price from CoinGecko (free, no API key needed)
3. Reads previous price from Google Sheets
4. Calculates % change
5. If change > 3% → sends Telegram alert with direction (📈/📉)
6. Logs every check to Google Sheets

## Time saved

No need to watch charts manually. Get notified only when it matters.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Schedule Trigger | Schedule | Every hour |
| Get BTC Price | HTTP Request | CoinGecko free API |
| Save Current Price | Set | Extracts price value |
| Get Previous Price | Google Sheets | Reads last logged price |
| Price Changed | IF | Checks if change > 3% |
| Send Alert | Telegram | Sends alert with % and direction |
| Update Price Log | Google Sheets | Appends new row with timestamp |

## Integrations

`n8n` · `HTTP Request` · `Google Sheets` · `Telegram` · `CoinGecko API`

## Setup

1. Import `workflow.json` into n8n
2. Set up Google Sheets credential (OAuth2)
3. Set up Telegram Bot credential
4. Replace `YOUR_SPREADSHEET_ID` with your Google Sheet ID
5. Replace `YOUR_CHAT_ID` with your Telegram chat ID
6. Activate the workflow

## Google Sheets structure

```
timestamp | price_usd | previous_price | change_percent | alert_sent
```

## Customize

- Change the `3` threshold in the IF node to any % you prefer
- Add ETH, SOL or other coins by duplicating and changing the CoinGecko URL
- Change `UAH` to any currency by editing the API URL parameter `vs_currencies`

## Screenshot

![workflow](screenshot.png)
