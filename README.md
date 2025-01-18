# ComplaintsApp

Two-interface complaint management system — citizens submit complaints through a Telegram bot, operators view and manage them through Django admin.

The bot guides users through a structured multi-step flow (FSM): name → phone number → complaint text. Each submitted complaint lands directly in the database, visible to operators in the admin panel.

---

## Architecture

```
Citizen
    │ Telegram
    ▼
aiogram 3 bot  (FSM: name → phone → text)
    │
    ▼ Django ORM (acreate)
PostgreSQL
    │
    ▼
Django Admin
    │
Operator
```

The bot and the admin share the same database. No API between them — the bot writes directly via the Django ORM using async `acreate()`.

---

## Bot flow

1. `/start` — greets the user in Uzbek, prompts to use `/compliant`
2. `/compliant` — begins the FSM:
   - **Step 1**: Asks for the user's name. A button pre-fills it from the Telegram profile (`full_name`).
   - **Step 2**: Asks for a phone number. A contact-share button lets the user send it without typing.
   - **Step 3**: Asks for the complaint text (free-form).
   - **Done**: `Complaint` record created, state cleared, confirmation sent.

State is stored in Redis with a 90-minute TTL — partial submissions expire automatically.

---

## Data model

```
Complaint
    ├── name          CharField(100)
    ├── phone_number  CharField(50)
    └── text          TextField
```

That's the full schema. Simple by design — operators need to see complaints, not manage complex workflows.

---

## Notable details

- **aiogram 3 FSM** with `RedisStorage` (not in-memory) — survives bot restarts, states expire after 90 minutes.
- **Contact-share button** on the phone step — no manual number typing.
- **Async ORM calls** (`Complaint.objects.acreate(...)`) — the bot event loop never blocks on DB writes.
- **Language**: bot interface is in Uzbek.
- Bot runs via Django management command (`python manage.py runbot`) — single process, no separate service needed.

---

## Stack

| Layer | Technology |
|---|---|
| Backend | Django 5.1 |
| Telegram bot | aiogram 3.17, aiohttp |
| State storage | Redis (FSM, 90-min TTL) |
| DB | PostgreSQL |
| Admin | Django default admin |

---

## Screenshots

| Screen | File |
|---|---|
| Complaint list in admin | `docs/screenshots/complaints-list.png` |

---

## Getting started

```bash
cp .env.copy .env
# set DJANGO_SECRET_KEY, BOT_TOKEN, POSTGRES_HOST/PORT/DB/USER/PASSWORD

python manage.py migrate
python manage.py runserver   # start Django (admin)

# in a separate terminal:
python manage.py runbot      # start Telegram bot
```

Admin at `http://127.0.0.1:8000/admin/`. Complaints appear there as users submit them through Telegram.
