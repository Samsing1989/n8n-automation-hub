# WF-30 — Social Media Crosspost

Автоматичний кросспостинг контенту з Telegram у Facebook, Instagram, LinkedIn та Twitter/X через єдиний n8n workflow.

## Що робить

1. Отримує повідомлення з Telegram-бота (текст, фото або JSON-файл)
2. Switch-нода визначає тип вхідних даних:
   - **JSON-документ** → завантажує файл, парсить структуру (`platform`, `text`, `image_url`, `hashtags`)
   - **Фото** → передає напряму
   - **Текст** → передає напряму
3. Форматує текст під ліміти кожної платформи
4. Публікує паралельно на всі платформи

## Платформи та ліміти

| Платформа | Ліміт символів |
|-----------|---------------|
| Facebook | 63 000 |
| Instagram | 2 200 |
| LinkedIn | 3 000 |
| Twitter/X | 280 |

## Структура JSON-файлу для відправки

```json
{
  "platform": ["facebook", "instagram", "linkedin", "twitter"],
  "text": "Текст публікації",
  "image_url": "https://example.com/image.jpg",
  "hashtags": "#n8n #automation"
}
```

## Налаштування

### Credentials

| Нода | Тип | Що вказати |
|------|-----|-----------|
| Telegram Trigger | Telegram API | Bot Token |
| Get Document Path / Download File | HTTP Request (URL) | `${TELEGRAM_BOT_TOKEN}` в URL |
| Post to Facebook | HTTP Request | Facebook Page Access Token + Page ID |
| Post to Instagram | HTTP Request | Instagram Graph API Token + IG User ID |
| Post to LinkedIn | HTTP Header Auth | LinkedIn OAuth Bearer Token + Person ID |
| Post to Twitter/X | HTTP Request | Twitter OAuth 2.0 Bearer Token |

### Змінні для заміни

```
${TELEGRAM_BOT_TOKEN}       — токен Telegram-бота
${YOUR_PAGE_ID}             — Facebook Page ID
${FACEBOOK_ACCESS_TOKEN}    — Facebook Page Access Token
${YOUR_IG_USER_ID}          — Instagram User ID
${INSTAGRAM_ACCESS_TOKEN}   — Instagram Graph API Token
${YOUR_LINKEDIN_PERSON_ID}  — LinkedIn Person URN
```

### Google Sheets (опційно)

Workflow можна розширити нодою Google Sheets для логування публікацій. Колонки: `timestamp | platform | status | post_id`.

## Схема потоку

```
Telegram Trigger
    └─► Switch
           ├─► [document] Get Document Path → Download File → Parse JSON Content
           ├─► [photo]    Format Post Text
           └─► [fallback] Format Post Text
                              └─► Post to Facebook
                                      └─► Post to Instagram
                                              └─► Post to LinkedIn
                                                      └─► Post to Twitter/X
```

## Стек

- **n8n** self-hosted
- Telegram Bot API
- Facebook Graph API v19.0
- Instagram Graph API v19.0
- LinkedIn UGC Posts API v2
- Twitter/X API v2

## Примітки

- Instagram підтримує публікацію через `image_url` (публічне посилання на зображення). Приватні URL не підходять.
- Twitter/X потребує окремого OAuth 2.0 додатку в Twitter Developer Portal.
- LinkedIn API потребує дозволів `w_member_social` у OAuth scopes.
- Workflow можна активувати тільки якщо Telegram-бот додано до чату або є приватним ботом.
