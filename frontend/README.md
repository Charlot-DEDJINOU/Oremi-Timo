# Frontend

This frontend is a React + Vite application for the AFG Oremi insurance experience. It provides the public website, the automobile quote/subscription flow, partner localisation, and the floating chatbot interface.

## Stack

- React 19
- Vite 6
- Tailwind CSS 4
- React Router
- Axios
- React Toastify
- React Leaflet + Leaflet
- Browser speech recognition and speech synthesis APIs

## Main pages

- `/`: marketing homepage and insurance offer landing page
- `/automobile`: multi-step automobile insurance flow with OCR-assisted document upload
- `/localisation`: nearby agency/partner finder with map and list views
- Floating chatbot available across the app

## Setup

```powershell
cd frontend
npm install
npm run dev
```

Development URL: `http://localhost:5173`

## Build

```powershell
npm run build
npm run preview
```

## Environment variables

Create a `.env` file in `frontend/` with:

```env
VITE_API_URI_BASE=http://127.0.0.1:8000/api
VITE_APP_URI_BASE=http://localhost:5173
```

## Project structure

```text
frontend/
|-- public/
|-- src/
|   |-- assets/
|   |-- components/
|   |-- data/
|   |-- hooks/
|   |-- services/
|   |-- styles/
|   |-- utils/
|   `-- views/
|-- index.html
|-- package.json
`-- vite.config.js
```

## Feature overview

### Homepage

- Product and service presentation
- Entry points to subscription and localisation flows
- Reusable header, footer, cards, and CTA components

### Automobile flow

- Uploads three document types:
  - `carte grise`
  - `CIP`
  - `permis`
- Sends files to `/cards/extract/`
- Auto-fills form fields from OCR results
- Includes speech guidance and keyboard accessibility helpers
- Simulates quote generation, validation, payment, and document delivery

Main file:
- [src/views/automobile/index.jsx](/c:/Users/CHARLOT/Personnel/Web/AFG%20Oremi/frontend/src/views/automobile/index.jsx)

### Localisation flow

- Manual and intelligent search modes
- Insurance-type filtering
- List and Leaflet map views
- Partner data with coordinates and direction links

Main file:
- [src/views/localisation/index.jsx](/c:/Users/CHARLOT/Personnel/Web/AFG%20Oremi/frontend/src/views/localisation/index.jsx)

### Chatbot

- Floating draggable/resizable widget
- Voice input and speech output
- Sends chat prompts to the backend generate endpoint

Main files:

- [src/components/chat/index.jsx](/c:/Users/CHARLOT/Personnel/Web/AFG%20Oremi/frontend/src/components/chat/index.jsx)
- [src/hooks/useChatStore.js](/c:/Users/CHARLOT/Personnel/Web/AFG%20Oremi/frontend/src/hooks/useChatStore.js)

## API integration

Axios is configured in [src/services/api.js](/c:/Users/CHARLOT/Personnel/Web/AFG%20Oremi/frontend/src/services/api.js).

Expected backend base URL:

- `VITE_API_URI_BASE=http://127.0.0.1:8000/api`

Current usage includes:

- `POST /chat/message/generate/`
- `POST /cards/extract/`

## Known caveats

- [src/services/api.js](/c:/Users/CHARLOT/Personnel/Web/AFG%20Oremi/frontend/src/services/api.js) currently includes a hardcoded bearer token. This should be replaced by real login/session storage.
- Some pages contain text encoding artifacts and large single-file components.
- The localisation page includes instructional/demo text that suggests React Leaflet installation even though the dependency is already present in `package.json`.
