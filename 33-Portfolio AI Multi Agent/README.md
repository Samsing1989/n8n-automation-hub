# WF-33 — Portfolio AI Multi-Agent

Telegram-бот з мульти-агентною архітектурою: три AI-агенти (Аналітик, Адвокат Диявола, Візіонер) паралельно аналізують запит користувача, Суддя синтезує фінальну відповідь. Розмовна пам'ять зберігається в Google Sheets.

## Що робить

1. Отримує запитання через Telegram
2. Завантажує історію розмови з Google Sheets (по `chat_id`)
3. Формує контекст із попередніх повідомлень
4. Запускає трьох агентів послідовно:
   - **Аналітик** — відповідає фактами (`[АНАЛІТИК]`, макс. 80 слів)
   - **Адвокат Диявола** — критикує відповідь аналітика (`[ДИЯВОЛ]`, макс. 80 слів)
   - **Візіонер** — пропонує нестандартний погляд у майбутнє
5. **Суддя** синтезує три думки в одну чисту відповідь
6. Відправляє відповідь у Telegram (Markdown)
7. Зберігає діалог у Google Sheets

## Агенти

| Агент | Роль | Стиль |
|-------|------|-------|
| Analyst | Факти та аналіз | Об'єктивний, конкретний |
| Devil | Критика | Провокаційний, скептичний |
| Visionary | Майбутнє та можливості | Творчий, нестандартний |
| Judge | Синтез | Чистий, без зайвих слів |

## Структура Google Sheets

Лист: **sheet2** (ai chat memory)

| Колонка | Опис |
|---------|------|
| `chat_id` | Telegram chat ID |
| `user_message` | Повідомлення користувача |
| `assistant_response` | Відповідь Judge |
| `timestamp` | Дата/час |

Фільтрація по `chat_id` дозволяє підтримувати окрему пам'ять для кожного користувача.

## Налаштування

### Credentials

| Нода | Тип | Що вказати |
|------|-----|-----------|
| Telegram Trigger | Telegram API | Bot Token |
| Get row(s) in sheet | Google Sheets OAuth2 | Google-акаунт |
| Append row in sheet | Google Sheets OAuth2 | Google-акаунт |
| Analyst / Devil / Visionary / Judge | HTTP Header Auth | `Authorization: Bearer YOUR_GROQ_API_KEY` |
| Telegram (Send) | Telegram API | Bot Token |

### Змінні для заміни

```
YOUR_GOOGLE_SHEET_ID    — ID таблиці Google Sheets (ai chat memory)
YOUR_GROQ_API_KEY       — ключ API Groq
```

### Groq Header Auth

```
Header Name:  Authorization
Header Value: Bearer YOUR_GROQ_API_KEY
```

## Схема потоку

```
Telegram Trigger
    └─► Edit Fields (Set: question, chat_id)
            └─► Get row(s) in sheet (фільтр по chat_id)
                    └─► Code in JavaScript1 (формує history[])
                            └─► Analyst (Groq)
                                    └─► Devil (Groq)
                                            └─► Visionary (Groq)
                                                    └─► Judge (Groq — синтез)
                                                            └─► Code in JavaScript (форматує відповідь)
                                                                    └─► Telegram (відправка, Markdown)
                                                                            └─► Append row in sheet (зберігає пам'ять)
```

## Системні промпти агентів

**Analyst:**
```
Ти АНАЛІТИК. Відповідай фактами. Починай з [АНАЛІТИК]. Макс 80 слів. Українською.
```

**Devil:**
```
Ти АДВОКАТ ДИЯВОЛА. Критикуй. Починай з [ДИЯВОЛ]. Макс 80 слів. Українською.
```

**Visionary:**
```
Ти — Візіонер. Твоє завдання: запропонувати нестандартний погляд на питання
користувача, дивлячись у майбутнє та шукаючи нові можливості. Відповідай українською.
```

**Judge:**
```
Ти — експертний помічник. Сформуй одну ідеальну відповідь на основі думок аналітиків.
ЗАБОРОНЕНО писати фрази типу: 'Ось відповідь', 'Мій аналіз'.
Одразу пиши суть відповіді українською мовою.
```

## Стек

- **n8n** self-hosted
- Telegram Bot API
- Google Sheets API
- Groq API (llama-3.3-70b-versatile) × 4 запити на повідомлення

## Примітки

- Відповідь відправляється з `parse_mode: Markdown` — можна використовувати `**bold**`, `*italic*`, `` `code` `` у промптах.
- Агенти запускаються **послідовно** (не паралельно): Devil бачить відповідь Analyst, Visionary — теж.
- При великій кількості запитів слід моніторити Groq rate limits (4 запити на одне повідомлення).
- Пам'ять не обмежена кількістю рядків — при потребі додай фільтр `LIMIT` по останнім N записам у Code in JavaScript1.
