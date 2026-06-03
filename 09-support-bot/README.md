# 09 — Support Bot

A Telegram support bot that automatically answers common customer questions about pricing, delivery, and returns. Unknown requests are forwarded to the manager.

## What it does

1. Listens for any incoming Telegram message
2. Routes by keyword detection (price / delivery / return)
3. Sends the relevant automated reply to the customer
4. Unknown topics → forwards full message to manager's Telegram

## Time saved

Handles 80%+ of repetitive support questions 24/7 without human involvement.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Telegram Trigger | Trigger | Listens for messages |
| Route by Topic | Switch | Keyword routing: price / deliv / retur |
| Reply Price | Telegram | Sends pricing info |
| Reply Delivery | Telegram | Sends delivery info |
| Reply Returns | Telegram | Sends return policy |
| Notify Manager (Fallback) | Telegram | Forwards unknown requests to manager |

## Integrations

`n8n` · `Telegram`

## Setup

1. Import `workflow.json` into n8n
2. Create a Telegram bot via [@BotFather](https://t.me/BotFather)
3. Set up Telegram Bot credential in n8n
4. Replace `YOUR_CHAT_ID` with your manager's Telegram chat ID
5. Edit reply texts in each Telegram node to match your actual pricing, delivery terms, and return policy
6. Activate the workflow

## Routing logic

| Keyword in message | Route |
|--------------------|-------|
| `price` | Reply Price |
| `deliv` | Reply Delivery |
| `retur` | Reply Returns |
| anything else | Notify Manager |

## Customize

- Add more topics by duplicating Switch rules and Telegram nodes
- Replace the fallback with an AI node (OpenAI / Groq) for smart free-form replies
- Add Google Sheets logging to track all support requests (see WF-02 for pattern)

## Screenshot

![workflow](screenshot.png)
