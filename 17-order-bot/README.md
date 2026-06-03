# 17 — Order Bot

A Telegram bot that collects orders step-by-step in a conversation: asks for name → address → product. Saves each step to Google Sheets, confirms with the client, and notifies the manager when the order is complete.

## What it does

1. Listens for Telegram messages
2. Checks current order state from Google Sheets
3. Routes to the correct step via Switch node
4. Collects: customer name → delivery address → product choice
5. Saves each answer to Sheets as the conversation progresses
6. Sends order confirmation to the client
7. Notifies manager with full order details

## Time saved

Replaces a manual order intake process. Orders are collected 24/7 without staff involvement.

## Nodes

| Node | Type | Purpose |
|------|------|---------|
| Receive Customer Message | Telegram Trigger | Listens for messages |
| Get Order State | Google Sheets | Reads current step for this user |
| Check Order Step | IF | Checks if order exists |
| Route Order Step | Switch | Routes to correct step handler |
| Ask Customer Name | Telegram | Step 1 prompt |
| Save Step 1 Name | Google Sheets | Saves name, sets step = 2 |
| Ask Address | Telegram | Step 2 prompt |
| Save Address | Google Sheets | Saves address, sets step = 3 |
| Ask Product | Telegram | Step 3 prompt |
| Update row in sheet | Google Sheets | Saves product, sets step = done |
| Read Final State | Google Sheets | Reads complete order |
| Confirm Client | Telegram | Order summary to client |
| Notify Manager | Telegram | Full order to manager |

## Integrations

`n8n` · `Telegram` · `Google Sheets`

## Setup

1. Import `workflow.json` into n8n
2. Set up Telegram Bot credential
3. Set up Google Sheets credential (OAuth2)
4. Create a Google Sheet with order state tracking
5. Replace `YOUR_SPREADSHEET_ID` with your sheet ID
6. Replace `YOUR_CHAT_ID` with your manager's Telegram chat ID
7. Activate the workflow

## Google Sheets structure

```
chat_id | name | address | product | step | timestamp
```
- `step`: `1` (waiting name) / `2` (waiting address) / `3` (waiting product) / `done`

## How state machine works

Each user has a row in Sheets identified by their `chat_id`. The bot reads the current `step` value and asks the next question accordingly. This pattern works for any multi-step conversation flow.

## Screenshot

![workflow](screenshot.png)
