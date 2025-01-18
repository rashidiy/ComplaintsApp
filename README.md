# ComplaintsApp

Complaint management system with a Django backend and a Telegram bot for operator notifications. Citizens can submit complaints; operators manage them through the admin panel and get notified via Telegram.

## Stack

- **Backend**: Django
- **Telegram bot**: aiogram
- **DB**: PostgreSQL (or SQLite for dev)

## Features

- Complaint submission and status tracking
- Django admin interface for operators
- Telegram bot — notifies operators on new complaints, supports command handling
- Clean app/core separation

## Project Structure

```
apps/
  models.py         — Complaint model
  views.py          — complaint endpoints
  admin.py          — admin panel config
  management/
    commands/       — Django management commands

tg_bot/
  handlers/
    commands/       — bot command handlers
  config.py         — bot token and settings

core/
  settings.py
  urls.py
```

## Getting Started

```bash
pip install -r req.txt
cp env.copy .env

python manage.py migrate
python manage.py runserver

# In a separate terminal, start the bot
python manage.py run_bot
```
