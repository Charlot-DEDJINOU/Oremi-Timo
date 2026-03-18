# Backend

This backend is a Django REST API for AFG Oremi. It handles authentication, chat conversations, LLM-backed responses, OCR document extraction, and export utilities for saved conversations.

## Stack

- Django 5
- Django REST Framework
- Simple JWT authentication
- drf-yasg for Swagger/ReDoc
- SQLite database in `backend/data/db.sqlite3`
- LangChain-based chat orchestration
- Groq-hosted Llama model support
- OCR via OCR.Space with local fallback tools

## Important folders

```text
backend/
|-- chatapp/           Main app: models, views, serializers, chat/OCR logic
|-- chatbot/           Django project settings and URL configuration
|-- data/              SQLite database
|-- Dockerfile
|-- docker-compose.yml
|-- manage.py
`-- requirements.txt
```

## Main backend capabilities

- User registration, login, profile, password reset, and password change
- Conversation, message, attachment, token usage, and saved prompt APIs
- Streaming chat generation endpoint
- OCR extraction for:
  - `grise_card`
  - `permis`
  - `id_card`
- Conversation export to PDF and Word

## Setup

### Local Python setup

```powershell
cd backend
python -m venv .venv
.venv\Scripts\activate
pip install --upgrade pip
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver
```

Backend URL: `http://127.0.0.1:8000`

### Docker setup

```powershell
cd backend
docker compose up --build
```

This starts the Django dev server on port `8000`.

## Required environment variables

Create a `.env` file in `backend/` with at least:

```env
GROQ_API_KEY=your_groq_key
OPENAI_API_KEY=your_openai_key
OCR_SPACE_API_KEY=your_ocr_space_key
EMAIL_USER=your_smtp_login
EMAIL_PASS=your_smtp_password
EMAIL_FROM=no-reply@example.com
```

## API overview

Base prefix: `/api/`

### Auth

- `POST /api/auth/register/`
- `POST /api/auth/login/`
- `GET /api/auth/profile/`
- `POST /api/auth/changepassword/`
- `POST /api/auth/send-reset-password-email/`
- `POST /api/auth/reset-password/<uid>/<token>/`

### Chat

- `POST /api/chat/message/generate/`
- `GET|POST /api/conversations/`
- `GET|POST /api/messages/`
- `GET|POST /api/attachments/`
- `GET /api/token-usages/`
- `GET|POST /api/saved-prompts/`

### OCR extraction

- `POST /api/cards/extract/`

Accepted request values:

- `file`: multipart file upload
- `url`: remote file URL
- `fileBase64`: base64 payload
- `file_type`: one of `grise_card`, `permis`, `id_card`

### Documentation

- Swagger: `http://127.0.0.1:8000/swagger/`
- ReDoc: `http://127.0.0.1:8000/redoc/`

## Key implementation notes

- Custom user model: [chatapp/models.py](/c:/Users/CHARLOT/Personnel/Web/AFG%20Oremi/backend/chatapp/models.py)
- Main API routes: [chatapp/urls.py](/c:/Users/CHARLOT/Personnel/Web/AFG%20Oremi/backend/chatapp/urls.py)
- Settings: [chatbot/settings.py](/c:/Users/CHARLOT/Personnel/Web/AFG%20Oremi/backend/chatbot/settings.py)
- Chat orchestration: [chatapp/chat_handler.py](/c:/Users/CHARLOT/Personnel/Web/AFG%20Oremi/backend/chatapp/chat_handler.py)
- OCR/document utilities: [chatapp/utils.py](/c:/Users/CHARLOT/Personnel/Web/AFG%20Oremi/backend/chatapp/utils.py)

## Known caveats

- `DEBUG = True`, `ALLOWED_HOSTS = ['*']`, and permissive CORS are enabled for development.
- [chatapp/chat_handler.py](/c:/Users/CHARLOT/Personnel/Web/AFG%20Oremi/backend/chatapp/chat_handler.py) prompts for `OPENAI_API_KEY` with `getpass` if it is missing, which is inconvenient for unattended runs.
- [chatapp/views.py](/c:/Users/CHARLOT/Personnel/Web/AFG%20Oremi/backend/chatapp/views.py) contains duplicated sections and is a good candidate for cleanup.
- OCR support depends on external services and local OCR fallbacks, so behavior can vary between environments.

