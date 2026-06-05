# WF-32 — AI Lead Qualifier

Автоматична кваліфікація лідів через webhook: Groq оцінює ліда по шкалі 0–10, гарячі ліди отримують 3 emails через Gmail, холодні — rejection. Паралельно створюються контакт і угода в HubSpot, дані зберігаються в Google Sheets, сповіщення надходять у Telegram.

## Що робить

1. Отримує POST-запит на webhook `/agency-lead`
2. Парсить дані ліда (`name`, `email`, `budget`, `deadline`, `project_type`, `message`)
3. Зберігає в Google Sheets (лист **Leads**)
4. Відправляє дані до Groq для AI-оцінки (score 0–10 + коментар)
5. Оновлює оцінку в Google Sheets
6. Створює Contact + Deal в HubSpot
7. Якщо `score > 7` (гарячий лід):
   - Gmail: Email 1 (Welcome) → затримка 1 день → Email 2 → затримка 1 день → Email 3
   - Telegram: сповіщення з деталями ліда та оцінкою AI
8. Якщо `score ≤ 7` (холодний лід):
   - Gmail: Cold Rejection email
   - Оновлення статусу в Google Sheets

## Webhook

```
POST https://YOUR_N8N_DOMAIN/webhook/agency-lead
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com",
  "budget": "2000",
  "deadline": "2025-08-01",
  "project_type": "Telegram Bot + CRM Integration",
  "message": "Need automation for my agency"
}
```

## Структура Google Sheets

Лист: **Leads**

| Колонка | Опис |
|---------|------|
| `lead_id` | Unix timestamp (унікальний ID) |
| `timestamp` | ISO дата/час |
| `name` | Ім'я ліда |
| `email` | Email |
| `budget` | Бюджет |
| `deadline` | Дедлайн |
| `project_type` | Тип проекту |
| `status` | `new` / `hot` / `cold` |
| `score` | AI-оцінка (0–10) |
| `ai_comment` | Коментар AI |

## Налаштування

### Credentials

| Нода | Тип | Що вказати |
|------|-----|-----------|
| Groq: Qualify Lead | HTTP Header Auth | `Authorization: Bearer YOUR_GROQ_API_KEY` |
| Append/Update row in sheet | Google Sheets OAuth2 | Google-акаунт |
| Sheets: Update Score | Google Sheets OAuth2 | Google-акаунт |
| HubSpot: Create Contact | HTTP Header Auth | `Authorization: Bearer YOUR_HUBSPOT_API_KEY` |
| HubSpot: Create Deal | HTTP Header Auth | HubSpot API Key |
| Gmail: Email 1–3 | Gmail OAuth2 | Google-акаунт з Gmail |
| Gmail: Cold Rejection | Gmail OAuth2 | Google-акаунт з Gmail |
| Send a text message | Telegram API | Bot Token |

### Змінні для заміни

```
YOUR_GOOGLE_SHEET_ID      — ID таблиці Google Sheets
YOUR_TELEGRAM_CHAT_ID     — твій особистий chat_id для сповіщень
YOUR_GROQ_API_KEY         — ключ API Groq
YOUR_HUBSPOT_API_KEY      — HubSpot Private App Token
```

### Groq промпт (Qualify Lead)

AI отримує всі поля ліда та повертає JSON:

```json
{
  "score": 8,
  "priority": "high",
  "ai_comment": "Великий бюджет, чіткий дедлайн, конкретний запит"
}
```

Поріг для гарячого ліда: `score > 7`

## Схема потоку

```
Webhook (POST /agency-lead)
    └─► Prepare Lead Data (Set)
            └─► Append or update row in sheet
                    └─► Groq: Qualify Lead (HTTP)
                            └─► Parse Groq Response (Code)
                                    └─► Sheets: Update Score
                                            └─► HubSpot: Create Contact
                                                    └─► HubSpot: Create Deal
                                                            └─► Is Hot Lead (IF score > 7)
                                                                   ├─► [true]  Gmail Email 1 → delay → Email 2 → delay → Email 3 → Telegram notify → Respond
                                                                   └─► [false] Gmail Cold Rejection → Update sheet status → Respond
```

## Стек

- **n8n** self-hosted
- Groq API (llama-3.3-70b-versatile)
- Google Sheets API
- HubSpot CRM API v3
- Gmail API
- Telegram Bot API

## Примітки

- HubSpot Deal Stage: `appointmentscheduled` (pipeline: `default`) — можна змінити під свій pipeline.
- Затримки між email (`Amount: 1 Day`) можна налаштувати у Wait-нодах.
- `lead_id` генерується як Unix timestamp (`$now.toMillis()`), тому унікальність гарантована.
- Для тестування webhook зручно використовувати `curl` або Postman.
