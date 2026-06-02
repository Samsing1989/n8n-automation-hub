# 04 — Currency Bot

A Telegram bot that converts any currency to UAH in real time. Send a message like `100 USD` and get the result instantly via the exchangerate-api.com free API.

## What it does

1. Listens for Telegram messages
2. Parses the input format: `[amount] [currency code]` (e.g. `250 EUR`)
3. Fetches live exchange rates from exchangerate-api.com
4. Calculates the UAH equivalent
5. Replies with the result in the same chat

## Time saved

Instant currency conversion without opening any app or browser.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Receive Telegram Message | Telegram Trigger | Listens for messages |
| Parse Currency Input | Code (JS) | Extracts amount + currency code |
| Fetch Exchange Rates | HTTP Request | Calls exchangerate-api.com |
| Calculate Converted Amount | Set | Computes UAH value |
| Send Conversion Result | Telegram | Replies with result |

## Integrations

`n8n` · `Telegram` · `HTTP Request` · `exchangerate-api.com` · `JavaScript`

## Setup

1. Import `workflow.json` into n8n
2. Create a Telegram bot via [@BotFather](https://t.me/BotFather)
3. Set up Telegram Bot credential in n8n
4. Activate the workflow

> No API key needed — exchangerate-api.com has a free tier with no auth required for basic use.

## Usage

Send a message to your bot in this format:
```
100 USD
250 EUR
50 GBP
```

The bot replies:
```
100 USD = 4150 UAH
```

## Customize

- Change `UAH` to any other target currency in the Set node
- Add error handling for unknown currency codes
- Add a `/rates` command to show top currencies at once

## Screenshot

![workflow](screenshot.png)
