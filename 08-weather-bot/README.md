# 08 — Weather Bot

Every morning at 8:00 AM fetches current weather via wttr.in (free, no API key), checks for rain, and sends a formatted Telegram forecast with a rain warning if needed.

## What it does

1. Triggers every day at 8:00 AM
2. Fetches weather data from wttr.in for your city
3. Extracts temperature, feels-like, description, max/min
4. Checks if forecast contains "rain"
5. Sends formatted message — with ☂️ umbrella warning if rain expected

## Time saved

Never forget an umbrella again. Zero manual effort.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Schedule Trigger | Schedule | Every day at 8:00 AM |
| Get Weather Data | HTTP Request | Fetches from wttr.in (free) |
| Extract Weather Fields | Set | Pulls temp, feelsLike, description, max/min |
| Check For Rain | IF | Checks if description contains "rain" |
| Send Weather With Rain Warning | Telegram | Message + ☂️ alert |
| Send Weather | Telegram | Regular forecast message |

## Integrations

`n8n` · `HTTP Request` · `Telegram` · `wttr.in`

## Setup

1. Import `workflow.json` into n8n
2. Set up Telegram Bot credential
3. Replace `YOUR_CITY` in the HTTP Request URL with your city name (e.g. `London`, `Kyiv`, `Berlin`)
4. Replace `YOUR_CHAT_ID` with your Telegram chat ID
5. Activate the workflow

## URL format

```
https://wttr.in/YOUR_CITY?format=j1
```

Works for any city worldwide. No registration or API key required.

## Customize

- Change trigger time in Schedule node
- Add wind speed: `$json.current_condition[0].windspeedKmph`
- Add humidity: `$json.current_condition[0].humidity`
- Add snow detection by checking for "snow" in description

## Screenshot

![workflow](screenshot.png)
