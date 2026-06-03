# n8n Workflow Library

A collection of production-ready n8n automation workflows — from simple lead capture to AI-powered multi-agent systems.

Built by **Pavlo Veresiuk** · AI Automation & n8n

---

## How to use any workflow

1. Open the workflow folder
2. Download `workflow.json`
3. In n8n: **Import** → paste or upload the JSON
4. Follow the setup steps in the `README.md`
5. Add your credentials and activate

---

## Workflows

### Automation

| # | Name | Description | Nodes |
|---|------|-------------|-------|
| 01 | [Lead Capture with Validation](./01-lead-capture/) | Webhook captures form leads, validates email, saves to Sheets, notifies via Telegram | Webhook · IF · Google Sheets · Telegram |
| 02 | [Telegram Support Bot](./02-support-bot/) | Routes /start /help /about commands, logs all interactions to Sheets | Telegram Trigger · Switch · Google Sheets |
| 03 | [Daily Morning Digest](./03-morning-digest/) | Every morning sends active tasks from Sheets to Telegram with urgent task detection | Schedule · Google Sheets · Code · Telegram |
| 04 | [Currency Bot](./04-currency-bot/) | Converts any currency to UAH in real time via Telegram | Telegram Trigger · HTTP Request · Code |
| 05 | [Lead Validator](./05-lead-validator/) | Advanced lead capture with email validation, HTTP responses, phone + source tracking | Webhook · IF · Google Sheets · Telegram |
| 06 | [Crypto Monitor](./06-crypto-monitor) | Checks BTC price hourly, sends alert if change exceeds 3% | Schedule · HTTP Request · IF · Telegram |
| 07 | [Client Broadcast](./07-client-broadcast) | Personalized weekly message to clients from Sheets via Telegram | Schedule · Google Sheets · Telegram |
| 08 | [Weather Bot](./08-weather-bot) | Daily weather forecast to Telegram at 7:30 via wttr.in | Schedule · HTTP Request · Telegram |
| 09 | [Support Bot](./09-support-bot) | Routes client requests by topic, responds automatically | Telegram Trigger · Switch · Telegram |
| 10 | [Uptime Monitor](./10-uptime-monitor) | Checks site availability every 5 min, alerts on downtime | Schedule · HTTP Request · IF · Telegram |
| 11 | [CRM Auto-fill](./11-crm-autofill) | New lead from form automatically creates HubSpot contact | Webhook · HTTP Request · HubSpot · Telegram |
| 12 | [News Digest](./12-faq-ai-bot) | Daily news collection by topic sent as HTML digest to email | Schedule · HTTP Request · RSS · Gmail |
| 13 | [Onboarding Flow](./13-news-digest) | New CRM client triggers automated 3-email series over 4 days | Webhook · Wait · Gmail · Telegram |
| 14 | [Instagram Leads](./14-onboarding-flow) | Instagram comments with keywords logged to CRM and sent to manager | Webhook · Instagram · IF · Google Sheets |
| 15 | [Order Bot](./15-instagram-leads) | Telegram bot collects orders step by step, saves to Sheets | Telegram Trigger · IF · Google Sheets |
| 16 | Weekly Report | Every Friday auto-generates and sends HTML business metrics report | Schedule · Google Sheets · Code · Gmail |
| 17 | Error Handler | Error monitoring: logs to Sheets + alerts via Telegram and Gmail | Error Trigger · Google Sheets · Telegram |
| 18 | Helpdesk | Multi-channel helpdesk: Telegram + email + form in one ticket system | Multiple Triggers · Google Sheets · Telegram |
| 19 | QA Feedback Bot | After delivery auto-collects client feedback, escalates low scores | Webhook · Wait · Telegram · IF · Google Sheets |
| 20 | Agency Automation | Full agency pipeline: from new lead to signed contract | Webhook · CRM · Gmail · DocuSign · Google Sheets |
| 21 | Social Media Crosspost | Publishes content simultaneously to Facebook, Instagram, LinkedIn, Twitter/X | Telegram Trigger · Switch · HTTP Request |

### AI Agency

| # | Name | Description | Nodes |
|---|------|-------------|-------|
| 22 | FAQ AI Bot | Telegram bot uses Groq to answer business questions automatically | Telegram Trigger · Groq · Google Sheets |
| 23 | Content Generator | Every Monday Groq auto-generates 5 social posts, saves to Sheets | Schedule · Groq · Code · Google Sheets |
| 24 | Recruiting Bot | AI analyzes resumes from Gmail, sends candidate score to HR | Gmail Trigger · Groq · Google Sheets · Telegram |
| 25 | AI Sales Analyst | Every Sunday Groq analyzes sales data, sends 3 specific recommendations | Schedule · Google Sheets · Groq · Gmail |
| 26 | Freelancer Hiring | Automated freelancer screening via platform API with AI profile scoring | Webhook · HTTP Request · Groq · Google Sheets |
| 27 | Auto Invoicing | Completed Trello task auto-generates invoice and sends to client | Webhook · Trello · Invoice API · Gmail |
| 28 | AI Lead Qualifier | Groq qualifies leads as hot/cold, auto-creates HubSpot deal, triggers follow-up | Webhook · Groq · HubSpot · Gmail |
| 29 | AI Memory Agent | AI agent stores conversation history in Sheets for context-aware replies | Telegram Trigger · Groq · Google Sheets |
| 30 | Portfolio AI Assistant | Telegram bot answers client questions about portfolio using Groq AI | Telegram Trigger · Groq · Google Sheets |
| 31 | Portfolio AI — Multi-Agent | 3 Groq agents (Analyst, Devil, Visionary) analyze in parallel, Judge summarizes | Telegram Trigger · Groq ×4 · Google Sheets |

### Dropshipping

| # | Name | Description | Nodes |
|---|------|-------------|-------|
| 32 | Business Dashboard | Google Sheets auto-updates from all data sources every 15 minutes | Schedule · HTTP Request · Google Sheets |
| 33 | Shop Automation | New WooCommerce order processed from confirmation to warehouse notification | Webhook · Google Sheets · Gmail · Telegram |

---

## Tech stack

- **n8n** — workflow automation platform (self-hosted)
- **Telegram** — notifications and bots
- **Google Sheets** — data storage and CRM
- **Groq** — AI inference (fast and free tier available)
- **JavaScript** — custom logic in Code nodes

---

## Connect

- LinkedIn: [linkedin.com/in/pavlo-veresiuk](https://linkedin.com/in/pavlo-veresiuk)

---

> All workflows are sanitized — no real credentials, IDs, or personal data. Replace placeholder values (`YOUR_CHAT_ID`, `YOUR_SPREADSHEET_ID`, etc.) before use.
