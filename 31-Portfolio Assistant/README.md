# WF-31 — Portfolio Assistant

Telegram-бот, який відповідає на запитання про портфоліо фрілансера на основі даних з Google Sheets. Використовує Groq (LLaMA 3.3 70B) для генерації відповідей.

## Що робить

1. Отримує запитання через Telegram
2. Читає Google Sheets з переліком проектів
3. Фільтрує тільки завершені проекти (`status = done`)
4. Формує системний промпт з контекстом портфоліо
5. Відправляє запит до Groq API
6. Повертає відповідь у Telegram

## Структура Google Sheets

Лист: **Projects**

| Колонка | Опис |
|---------|------|
| `title` | Назва проекту |
| `description_uk` | Опис українською |
| `status` | Статус (`done` / `in_progress`) |

Workflow читає тільки рядки зі статусом `done`.

## Налаштування

### Credentials

| Нода | Тип | Що вказати |
|------|-----|-----------|
| Receive Portfolio Question | Telegram API | Bot Token |
| Read Projects Sheet | Google Sheets OAuth2 | Google-акаунт з доступом до таблиці |
| Ask Groq About Portfolio | HTTP Header Auth | `Authorization: Bearer YOUR_GROQ_API_KEY` |
| Send Answer Telegram | Telegram API | Bot Token (той самий) |

### Змінні для заміни

```
YOUR_GOOGLE_SHEET_ID    — ID таблиці Google Sheets (з URL)
YOUR_TELEGRAM_CHAT_ID   — chat_id для відправки відповідей
YOUR_GROQ_API_KEY       — ключ API Groq (https://console.groq.com)
```

### Groq API Header

```
Header Name:  Authorization
Header Value: Bearer YOUR_GROQ_API_KEY
```

## Схема потоку

```
Receive Portfolio Question (Telegram Trigger)
    └─► Read Projects Sheet (Google Sheets)
            └─► Filter Completed Projects (IF: status = done)
                    └─► Build Portfolio Context (Code)
                            └─► Ask Groq About Portfolio (HTTP Request)
                                    └─► Extract AI Answer (Code)
                                            └─► Send Answer Telegram
```

## Системний промпт

```
Ти — асистент фрілансера Павла. Його напрацювання: {список проектів}
```

Список формується динамічно: `title: description_uk | title: description_uk | ...`

## Стек

- **n8n** self-hosted
- Telegram Bot API
- Google Sheets API
- Groq API (llama-3.3-70b-versatile, max_tokens: 1000)

## Примітки

- Відповідь повертається у той самий чат, з якого прийшло запитання (якщо замінити хардкодований `chatId` на `$json.message.chat.id`).
- Можна розширити: додати фільтр по категорії проекту або підтримку кількох мов.
