# WF-29 — Agency Automation Suite

Повний набір n8n workflows для автоматизації агентства: від захоплення ліда до збору відгуків. Всі 6 воркфлоу працюють з єдиною Google Sheets базою — **Agency CRM**.

---

## Архітектура системи

```
[Сайт / Telegram]
       │
       ▼
 29a Lead Capture ──► Google Sheets (LEADS) ──► HubSpot CRM ──► Telegram notify
       │
       ▼
 29b Project Tracker ──► Google Sheets (PROJECTS) ──► Telegram bot commands
       │
       ▼
 29c Weekly Report ──► [щопонеділка 09:00] ──► Telegram звіт
       │
 29d Price Calculator ──► Telegram бот ──► Google Sheets (LEADS)
       │
 29e Follow Up ──► [щодня 10:00] ──► Telegram нагадування
       │
 29f Feedback System ──► [тригер: завершений проект] ──► Gmail → Google Sheets update
```

---

## Google Sheets — Agency CRM

Одна таблиця (`YOUR_GOOGLE_SHEET_ID`) з чотирма листами:

| Лист | Використовується в | Призначення |
|------|-------------------|-------------|
| **LEADS** (`gid=0`) | 29a, 29e | Ліди з усіх джерел |
| **PROJECTS** | 29b, 29c, 29f | Активні та завершені проекти |
| **Projects & Feedback** | 29f | Тригер для запиту відгуків |
| **LEADS** (калькулятор) | 29d | Ліди з Telegram /consult |

### Структура LEADS

| Колонка | Опис |
|---------|------|
| `ID` | `LEAD-{timestamp}` |
| `Ім'я` | Ім'я контакту |
| `Email` | Email |
| `Telegram` | Telegram username |
| `Компанія` | Назва компанії |
| `Бюджет` | Бюджет |
| `Джерело` | `website` / `Telegram /consult` |
| `Статус` | `Новий` / `В роботі` / `Завершено` |
| `Дата` | `dd.MM.yyyy HH:mm` |

### Структура PROJECTS

| Колонка | Опис |
|---------|------|
| `ID` | `PROJ-{timestamp}` |
| `Клієнт` | Назва клієнта |
| `Тип проекту` | Тип роботи |
| `Статус` | `В роботі` / `Завершено` |
| `Дедлайн` | Дата дедлайну |
| `Оплата` | Сума (числове значення) |
| `Workflow URL` | Посилання на n8n workflow |
| `Завершено` | Дата завершення |

---

## Воркфлоу

### 29a — Agency Lead Capture

Захоплює ліди через POST webhook, зберігає в Google Sheets, створює контакт і угоду в HubSpot, надсилає Telegram-сповіщення.

**Webhook:** `POST https://YOUR_N8N_DOMAIN/webhook/agency-lead`

```json
{
  "name": "Іван Петренко",
  "email": "ivan@example.com",
  "telegram": "@ivan",
  "company": "ТОВ Ромашка",
  "budget": "1500",
  "source": "website"
}
```

**Схема:**
```
Webhook → Normalize Lead Data → Save Lead to Sheets
       → Create HubSpot Deal → Upsert HubSpot Contact
       → Notify Manager Telegram
```

---

### 29b — Agency Project Tracker

Telegram-бот для управління проектами через команди.

**Команди:**

| Команда | Дія |
|---------|-----|
| `/newproject Клієнт \| Тип \| Дедлайн \| Оплата` | Створити проект |
| `/status` | Показати активні проекти |
| `/update {номер_рядка} \| {новий статус}` | Оновити статус |
| `/report` | Тижневий звіт по проектах |

**Приклад:**
```
/newproject ТОВ Ромашка | Telegram Bot | 25.12.2024 | 15000
/update 3 | Завершено
```

**Схема:**
```
Telegram Trigger → Route Project Action (Switch)
  ├─► [/newproject] Prepare New Project → Save New Project → (підтвердження)
  ├─► [/status]     Read Project Data → Format Status Report → Send Status Telegram
  ├─► [/update]     Prepare Project Update → Update Project Sheets → Confirm Update Telegram
  ├─► [/report]     Read Weekly Data → Format Weekly Report → Send Weekly Report
  └─► [fallback]    Send Help Message
```

---

### 29c — Agency Weekly Report

Автоматичний тижневий звіт. Запускається кожного понеділка о 09:00 (cron: `0 9 * * 1`).

Рахує з листа PROJECTS:
- кількість активних проектів (`Статус = В роботі`)
- кількість завершених (`Статус = Завершено`)
- загальну суму оплат по завершених

**Схема:**
```
Schedule Trigger (кожен пн 09:00)
    → Read Agency Metrics (Google Sheets: PROJECTS)
    → Build Weekly Report (Code: підрахунок метрик)
    → Send Report Telegram
```

---

### 29d — Agency Price Calculator

Telegram-бот для розрахунку вартості послуг. Без жодних зовнішніх API — вся логіка в Code-нодах.

**Команди та flow:**

| Введення | Дія |
|----------|-----|
| `/price` | Показати меню з 6 типами послуг |
| `1` – `6` | Показати орієнтовну ціну |
| `/consult` | Запросити форму для збору контакту |
| Ім'я + телефон + email (3 рядки) | Зберегти ліда → сповістити менеджера |

**Прайс-лист (вбудований у Code-ноду):**

| # | Послуга | Ціна | Термін |
|---|---------|------|--------|
| 1 | Telegram бот | $150–300 | 3–5 днів |
| 2 | CRM інтеграція | $200–400 | 5–7 днів |
| 3 | E-commerce | $350–700 | 7–10 днів |
| 4 | AI агент | $250–500 | 5–8 днів |
| 5 | Соцмережі | $150–300 | 3–5 днів |
| 6 | Комплексне рішення | $500–1500 | 14–21 день |

**Схема:**
```
Telegram Trigger → Route Service Type (Switch)
  ├─► [/price або 1-6] Calculate Basic Price → Send Basic Quote
  ├─► [/consult]       Calculate Consult Price → Send Basic Quote
  ├─► [3 рядки тексту] Prepare Custom Quote → Log Quote to Sheets → Send Custom Quote
  └─► [fallback]       Send Default Message
```

---

### 29e — Agency Follow Up

Щоденна перевірка (о 10:00) лідів зі статусом `Новий`, які не отримали відповідь більше 2 днів. Надсилає Telegram-нагадування з переліком.

**Логіка перевірки дати:**
- Підтримує формати `dd.MM.yyyy` та ISO
- Порівнює з `today - 2 days` (без урахування часу)

**Схема:**
```
Schedule Trigger (щодня 10:00)
    → Read Pending Leads (фільтр: Статус = Новий)
    → Check Follow Up Due (Code: фільтр по даті)
    → Filter Due Leads (IF: є ліди > 0)
       ├─► [true]  Prepare Follow Up Message → Send Follow Up Telegram
       └─► [false] Skip No Action Needed
```

---

### 29f — Agency Feedback System

Автоматичний запит відгуку після завершення проекту. Тригер — новий рядок або зміна в листі `Projects & Feedback`.

**Логіка:**
1. Google Sheets Trigger (polling кожну хвилину) — стежить за листом
2. Перевіряє: `status = true` AND `feedback_requested = true`
3. Чекає **3 дні** (Wait-нода)
4. Перевіряє що відгук ще не надіслано (`feedback_sent ≠ true`)
5. Читає дані клієнта з листа PROJECTS по `project_id`
6. Надсилає email через Gmail
7. Оновлює `feedback_sent = true` в Google Sheets

**Поля в Projects & Feedback:**

| Поле | Тип | Опис |
|------|-----|------|
| `status` | boolean | `true` = проект завершено |
| `feedback_requested` | boolean | `true` = запит дозволено |
| `feedback_sent` | boolean | `true` = вже надіслано |
| `project_id` | string | ID для lookup в PROJECTS |
| `client_email` | string | Email клієнта |
| `feedback_form_link` | string | Посилання на форму відгуку |

**Схема:**
```
Watch Order Completed (Google Sheets Trigger)
    → Check Delivery Status (IF: status=true AND feedback_requested=true)
    → Wait Feedback Delay (3 дні)
    → Verify Not Sent Yet (IF: feedback_sent ≠ true)
    → Read Client Data (lookup по project_id)
    → Send Feedback Request (Gmail)
    → Mark Feedback Sent (update Google Sheets)
```

---

## Налаштування

### Credentials (спільні для всіх воркфлоу)

| Тип | Нода | Що вказати |
|-----|------|-----------|
| **Telegram API** | всі Telegram-ноди | Bot Token з @BotFather |
| **Google Sheets OAuth2** | всі Google Sheets-ноди | Google-акаунт з доступом до таблиці |
| **Google Sheets Trigger OAuth2** | 29f: Watch Order Completed | Окремий OAuth2 (або той самий акаунт) |
| **HubSpot App Token** | 29a: Create Deal, Upsert Contact | Private App Token з HubSpot |
| **Gmail OAuth2** | 29f: Send Feedback Request | Google-акаунт з Gmail |

### Змінні для заміни в усіх файлах

```
YOUR_GOOGLE_SHEET_ID      — ID таблиці Agency CRM (з URL Google Sheets)
YOUR_TELEGRAM_CHAT_ID     — твій особистий chat_id для сповіщень менеджера
YOUR_TELEGRAM_BOT_TOKEN   — токен бота з @BotFather
YOUR_HUBSPOT_API_KEY      — HubSpot Private App Token
```

### Порядок активації

Рекомендований порядок увімкнення воркфлоу:

```
1. 29a — Lead Capture      (спочатку — дані починають надходити)
2. 29b — Project Tracker   (після першого ліда)
3. 29c — Weekly Report     (тло, не критично)
4. 29d — Price Calculator  (якщо потрібен Telegram-прайс)
5. 29e — Follow Up         (після накопичення лідів)
6. 29f — Feedback System   (після перших завершених проектів)
```

---

## Стек

- **n8n** self-hosted
- Telegram Bot API
- Google Sheets API (OAuth2)
- HubSpot CRM API v3 (App Token)
- Gmail API (OAuth2)

## Примітки

- **29b `/update`** використовує номер рядка, не ID проекту — будь уважний при видаленні рядків.
- **29f polling** кожну хвилину — може генерувати навантаження на Google Sheets API при великій таблиці. Рекомендується переключити на `everyHour` у продакшені.
- **29d прайс** захардкоджений у Code-ноді `Calculate Basic Price` — редагуй безпосередньо там.
- Всі воркфлоу зберігають `active: false` — активуй вручну після налаштування credentials.
