# AFG Oremi

AFG Oremi is a full-stack insurance platform with a React frontend and a Django REST backend. The project focuses on a digital insurance experience for AFG Assurances Benin, including automobile quote flows, nearby partner discovery, chatbot assistance, and OCR-based document extraction.

## Project structure

```text
.
|-- backend/   Django API, chat engine, OCR/document extraction
`-- frontend/  React + Vite web application
```

## Main features

- Marketing and product landing pages for insurance offers
- Multi-step automobile subscription and quote experience
- OCR upload flow for `carte grise`, `CIP`, and driving license documents
- Embedded chatbot connected to the backend LLM endpoints
- Partner localisation page with Leaflet map integration
- Authentication and conversation management APIs

## Tech stack

- Frontend: React 19, Vite, Tailwind CSS, Axios, React Router, React Leaflet
- Backend: Django 5, Django REST Framework, Simple JWT, drf-yasg
- AI/OCR: Groq Llama, OpenAI-compatible LangChain integration, OCR.Space fallback, Tesseract/PyPDF2 local fallback
- Database: SQLite by default
- Containerization: Docker Compose for backend development

## Getting started

### 1. Backend

See [backend/README.md](/c:/Users/CHARLOT/Personnel/Web/AFG%20Oremi/backend/README.md) for the full backend setup.

Quick start:

```powershell
cd backend
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver
```

The API runs on `http://127.0.0.1:8000`.

### 2. Frontend

See [frontend/README.md](/c:/Users/CHARLOT/Personnel/Web/AFG%20Oremi/frontend/README.md) for the frontend guide.

Quick start:

```powershell
cd frontend
npm install
npm run dev
```

The frontend runs on `http://localhost:5173`.

## Environment variables

The project does not currently ship with `.env.example` files, but these values are expected by the codebase.

### Backend

- `GROQ_API_KEY`: used by the custom Llama integration
- `OPENAI_API_KEY`: used by OpenAI-compatible LangChain models
- `OCR_SPACE_API_KEY`: optional, for OCR.Space requests
- `EMAIL_USER`: SMTP username
- `EMAIL_PASS`: SMTP password
- `EMAIL_FROM`: sender address

### Frontend

- `VITE_API_URI_BASE`: base URL for the backend API, usually `http://127.0.0.1:8000/api`
- `VITE_APP_URI_BASE`: frontend base URL used by helper redirects

## API entry points

- API root: `http://127.0.0.1:8000/api/`
- Swagger: `http://127.0.0.1:8000/swagger/`
- ReDoc: `http://127.0.0.1:8000/redoc/`

Notable endpoints:

- `/api/auth/register/`
- `/api/auth/login/`
- `/api/chat/message/generate/`
- `/api/conversations/`
- `/api/messages/`
- `/api/cards/extract/`

## Notes for contributors

- The frontend currently contains a hardcoded JWT in [frontend/src/services/api.js](/c:/Users/CHARLOT/Personnel/Web/AFG%20Oremi/frontend/src/services/api.js). That should be replaced with a proper auth flow before production use.
- The backend currently allows all hosts and all CORS origins in development settings.
- Some source files contain duplicated code and text encoding issues; the new READMEs document the current behavior, not an idealized target architecture.

