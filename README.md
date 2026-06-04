# Django Films v2

A Django 5 web application and REST API for browsing and managing films, people (cast & crew), and related data, paired with a React single-page frontend. The backend is built around Django REST Framework with JWT and API-key authentication, real-time messaging via Django Channels, OpenAPI documentation, and Cloudinary-backed media. It is organized into focused Django apps (films, people, reviews, store, users, activity, communications) and ships with Docker / Docker Compose for local orchestration of Postgres and Redis.

## Features

- **Films & people catalog** — `films` app models cover Films, Genres, People, Film roles, Production companies, Countries, and their relationships (e.g. `FilmAndGenre`, `FilmAndPerson`, `FilmAndProductionCompany`).
- **REST API** — Django REST Framework endpoints for films, films-by-person, persons, reviews, activity, store, and users, with page-number pagination.
- **Authentication** — JWT auth (`rest_framework_simplejwt`) with token obtain/refresh endpoints, plus API-key permissions (`rest_framework_api_key`).
- **API documentation** — OpenAPI schema and Swagger UI via `drf-spectacular` (`/api/schema/`, `/api/docs/`).
- **Real-time communications** — `comms` app with Django Channels consumers, routing, and a chat template, served over ASGI (Daphne) with a Redis channel layer.
- **Friends activity feed** — `activity` app serializing friends' activity.
- **Reviews & store** — dedicated `reviews` and `store` apps.
- **Media storage** — Cloudinary integration (`django-cloudinary-storage`) for posters/backdrops/images.
- **CORS & custom middleware** — `django-cors-headers` plus custom middleware (React-access restriction, request timing).
- **React frontend** — Vite + React 19 SPA in `frontend/` with routing, forms (login/register), layout, and home views.

## Tech Stack

**Backend**
- Python / Django 5.2
- Django REST Framework 3.16 (+ SimpleJWT, API-Key, drf-spectacular)
- Django Channels 4 + channels-redis (ASGI via Daphne)
- PostgreSQL (psycopg2) and Redis
- Cloudinary, django-cors-headers, django-environ, django-extensions, Faker

**Frontend** (`frontend/`)
- React 19 + Vite 6
- React Router 7, Zustand, React Hook Form, Axios
- Tailwind CSS 4, Headless UI, Framer Motion, lucide-react / react-icons

**Infra**
- Docker + Docker Compose (Postgres 15, Redis 6, Django/Daphne web)

## Getting Started

### Prerequisites

- Python 3 and pip
- Node.js (with npm) for the frontend
- PostgreSQL and Redis (or use Docker Compose, which provides both)

### Run with Docker Compose

```bash
docker-compose up --build
```

This starts Postgres, Redis, and the Django app (runs migrations, loads fixtures, collects static, and serves with Daphne on port 8000).

### Run the backend locally

```bash
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver
```

The API documentation is available at `/api/docs/` and the Django admin at `/admin/`.

### Run the frontend locally

```bash
cd frontend
npm install
npm run dev
```

> Note: database, Cloudinary, and other settings live in `pDjangoFilmsV2/settings.py` and environment configuration; review and adjust them (and the `.env` files) for your environment.

## Project Structure

```
web-django-films-v2/
├── manage.py
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
├── pDjangoFilmsV2/          # Project config: settings, urls, asgi, wsgi
├── films/                   # Films & people: models, serializers, views, urls
├── reviews/                 # Reviews app
├── store/                   # Store app
├── users/                   # Users app
├── activity/                # Friends activity feed
├── comms/                   # Real-time chat (Channels consumers, routing)
├── integrations/
│   └── api_manager/         # API routing / management
├── core/                    # Shared helpers, mixins, middlewares, base models
└── frontend/                # React + Vite single-page app
    └── src/
        ├── App.jsx
        └── components/      # forms, home, layout
```
