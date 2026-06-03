# 16 — Content Generator

Every Monday at 9:00 AM uses Groq AI to generate 5 social media posts, saves them to Google Sheets content plan, and notifies the manager for review.

## What it does

1. Triggers every Monday at 9:00 AM
2. Builds a prompt for Groq AI (5 varied post types)
3. Sends prompt to Groq (llama-3.3-70b-versatile)
4. Parses response — splits by `###` separator into 5 posts
5. Assigns publish dates Mon–Fri of current week
6. Saves to Google Sheets with status "Pending review"
7. Sends Telegram notification with link to the sheet

## Time saved

~2–3 hours/week of manual content creation. Every Monday starts with a ready content plan.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Schedule Trigger | Schedule | Every Monday 9:00 AM |
| Set Content Prompt | Set | Builds AI prompt |
| Generate Posts Groq | HTTP Request | Calls Groq API |
| Parse Posts Array | Code (JS) | Splits response into 5 posts with dates |
| Save Posts to Sheets | Google Sheets | AppendOrUpdate content plan |
| Notify Manager Telegram | Telegram | Link to review sheet |

## Integrations

`n8n` · `Groq API` · `Google Sheets` · `Telegram` · `JavaScript`

## Setup

1. Import `workflow.json` into n8n
2. Get a free Groq API key at [console.groq.com](https://console.groq.com)
3. Add it as HTTP Header Auth credential: header `Authorization`, value `Bearer YOUR_KEY`
4. Set up Google Sheets credential (OAuth2)
5. Set up Telegram Bot credential
6. Replace `YOUR_SPREADSHEET_ID` with your Google Sheet ID
7. Replace `YOUR_SPREADSHEET_URL` in the Telegram node with your sheet URL
8. Replace `YOUR_CHAT_ID` with your Telegram chat ID
9. Edit the prompt in the Set node to match your business/niche
10. Activate the workflow

## Google Sheets structure

```
№ | Publish Date | Post Text | Status
```
- `Status`: `Pending review` / `Approved` / `Needs revision`

## Customize

- Change `llama-3.3-70b-versatile` to any other Groq model
- Edit the prompt language and tone to match your brand voice
- Add a second workflow that auto-posts approved content to social media

## Screenshot

![workflow](screenshot.png)
