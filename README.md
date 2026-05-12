# DashGallery

DashGallery is a full-stack image gallery starter built with a Laravel API backend and a Vue 3 frontend. The project is set up for SPA authentication with Laravel Sanctum and includes the core screens for registration, login, upload, and a personal image gallery.

## Tech Stack

- **Backend:** Laravel 11, PHP 8.2+, Laravel Sanctum, Laravel Breeze auth controllers
- **Frontend:** Vue 3, Vite 6, Vue Router, Axios, Tailwind CSS 4
- **UI utilities:** Headless UI and Heroicons
- **Database:** MySQL by default through the Laravel `.env.example`

## Features

- User registration and login
- Cookie-based SPA authentication with Sanctum
- Protected dashboard layout with desktop and mobile navigation
- Upload and My Images routes ready for gallery functionality
- Axios client configured for CSRF and credentialed requests
- Laravel feature tests for authentication flows

## Project Structure

```text
DashGallery/
|-- backend/      # Laravel API, auth routes, config, database migrations, tests
`-- frontend/     # Vue/Vite SPA, routes, layouts, pages, Axios client
```

## Requirements

- PHP 8.2 or newer
- Composer
- Node.js and npm
- MySQL or another Laravel-supported database

## Backend Setup

```bash
cd backend
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate
php artisan serve
```

By default, the backend runs at:

```text
http://localhost:8000
```

For local SPA authentication, make sure these values are present in `backend/.env`:

```env
APP_URL=http://localhost:8000
FRONTEND_URL=http://localhost:5173
SANCTUM_STATEFUL_DOMAINS=localhost:5173,127.0.0.1:5173,localhost:8000,127.0.0.1:8000
SESSION_DOMAIN=localhost
```

Update the database values in `backend/.env` to match your local MySQL setup before running migrations.

## Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

The frontend runs at:

```text
http://localhost:5173
```

The current frontend environment file points API requests to the Laravel server:

```env
VITE_API_BASE_URL=http://localhost:8000
```

## Running Both Apps

Open two terminals:

```bash
cd backend
php artisan serve
```

```bash
cd frontend
npm run dev
```

Then visit `http://localhost:5173`.

## API Notes

The frontend uses Laravel Sanctum's browser session flow:

1. Request `GET /sanctum/csrf-cookie`
2. Submit credentials to `POST /login` or `POST /register`
3. Access authenticated endpoints with cookies included
4. Sign out with `POST /logout`

Available auth routes are defined in `backend/routes/auth.php`. The authenticated user endpoint is available at `GET /api/user` with the `auth:sanctum` middleware.

## Useful Commands

Backend:

```bash
php artisan test
php artisan migrate
php artisan route:list
```

Frontend:

```bash
npm run dev
npm run build
npm run preview
```

## Next Improvements

- Build the actual image upload form on the Upload page
- Add backend image storage, validation, and gallery APIs
- Render uploaded images on the My Images page
- Replace placeholder user data with the authenticated user profile
- Add loading, empty, and error states for auth and gallery workflows
