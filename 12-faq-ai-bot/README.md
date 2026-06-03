# 12 — FAQ AI Bot

Telegram bot powered by Groq AI that automatically answers customer questions. If the AI is confident — replies directly. If not — escalates to the manager and logs the gap.

## What it does

1. Listens for any Telegram message
2. Sends the question to Groq AI with a business FAQ context
3. Checks AI confidence in the answer
4. **Known answer** → sends reply to user + logs to Sheets
5. **Unknown** → notifies manager + tells user "we'll get back to you" + logs to Sheets

## Time saved

Handles FAQ 24/7 without human involvement. Manager only gets notified for genuinely new questions.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Telegram Trigger | Trigger | Listens for messages |
| Groq AI Request | HTTP Request | Calls Groq API with question |
| Extract AI Response | Set | Pulls answer text |
| Check If AI Known | IF | Confidence check |
| Send Answer to User | Telegram | Sends AI answer |
| Log to Sheets (Known) | Google Sheets | Logs answered questions |
| Notify Manager | Telegram | Escalates unknown questions |
| "Escalated" to User | Telegram | Tells user we'll follow up |
| Log to Sheets (Escalated) | Google Sheets | Logs escalated questions |

## Integrations

`n8n` · `Telegram` · `Groq API` · `Google Sheets`

## Setup

1. Import `workflow.json` into n8n
2. Create a free Groq API key at [console.groq.com](https://console.groq.com)
3. Add Groq API key as HTTP Header Auth credential in n8n
4. Set up Telegram Bot credential
5. Set up Google Sheets credential (OAuth2)
6. Replace `YOUR_SPREADSHEET_ID` with your Google Sheet ID
7. Replace `YOUR_CHAT_ID` with your manager's Telegram chat ID
8. Edit the system prompt in the Groq node to describe your business FAQ
9. Activate the workflow

## Google Sheets structure

```
timestamp | username | question | answer | status
```
- `status`: `answered` / `escalated`

## Customize

- Swap Groq for OpenAI by changing the HTTP Request URL and auth
- Add a Google Sheets FAQ knowledge base that feeds into the prompt
- Lower the confidence threshold to escalate more often

## Screenshot

![workflow](screenshot.png)
