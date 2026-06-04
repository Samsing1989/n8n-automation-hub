# WF-24 — AI Product Generator

**Category:** Dropshipping / WooCommerce  
**Level:** ⚡ Expert  
**Status:** Done

## What it does
Automatically generates SEO-optimized product descriptions in German 
for WooCommerce store using Groq AI. Finds new products from CJ 
Dropshipping and publishes them with AI-written content.

## Nodes
- Schedule Trigger / Telegram Trigger — weekly or on demand
- HTTP Request (CJ Search Products) — finds new products
- Groq AI (Generate Description) — writes SEO title + description (DE)
- Code (Build Product Data) — formats JSON for WooCommerce
- HTTP Request (WC Create Product) — publishes to WooCommerce
- Google Sheets (Product Log) — logs new products
- Telegram (Admin Notify) — sends link for review

## Tech Stack
n8n · CJ Dropshipping API · Groq AI · WooCommerce · Google Sheets · Telegram

## Result
Product catalog grows automatically. Groq writes proper German 
descriptions without manual work.
