# 21 — Recruiting Bot

Monitors Gmail for emails with "Resume" in the subject, extracts the resume text, scores the candidate with Groq AI (1–10), saves to Google Sheets, and notifies HR via Telegram — with different messages for strong and weak candidates.

## What it does

1. Polls Gmail every minute for emails matching "Resume"
2. Extracts sender name, email, and body text
3. Sends resume to Groq AI for scoring (experience 40%, skills 30%, education 20%, clarity 10%)
4. Saves candidate + score to Google Sheets
5. Score ≥ 7 → 🔔 Recommended for interview alert to HR
6. Score < 7 → ❌ Not recommended alert to HR

## Time saved

Eliminates manual resume screening. HR only reviews pre-filtered candidates.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Gmail Trigger | Gmail Trigger | Polls for emails with "Resume" |
| Extract Resume Text | Code (JS) | Extracts sender name, email, body |
| Analyze Resume Groq | HTTP Request | Groq AI scores the resume |
| Prepare Candidate Score | Set | Parses AI JSON response |
| Save Score to Sheets | Google Sheets | Logs candidate + score |
| Check Score Threshold | IF | Routes by score ≥ 7 |
| Notify HR High Score | Telegram | Recommended for interview |
| Notify HR Low Score | Telegram | Not recommended |

## Integrations

`n8n` · `Gmail` · `Groq API` · `Google Sheets` · `Telegram` · `JavaScript`

## Setup

1. Import `workflow.json` into n8n
2. Set up Gmail credential (OAuth2)
3. Get a free Groq API key at [console.groq.com](https://console.groq.com)
4. Add as HTTP Header Auth: `Authorization: Bearer YOUR_KEY`
5. Set up Google Sheets credential (OAuth2)
6. Set up Telegram Bot credential
7. Replace `YOUR_SPREADSHEET_ID` with your candidates sheet ID
8. Replace `YOUR_CHAT_ID` with your HR Telegram chat ID
9. Change the Gmail filter keyword if needed (`"Resume"` → your language)
10. Activate the workflow

## Google Sheets structure

```
Date | Name | Email | Score | AI Comment | Status
```
- `Status`: `New` / `Interview scheduled` / `Rejected`

## Customize

- Change the score threshold (7) in the IF node
- Add auto-reply email to candidates (Gmail node)
- Edit the AI prompt to match your specific job requirements
- Add more scoring criteria in the system prompt

## Screenshot

![workflow](screenshot.png)
