# DashGallery

DashGallery is a full-stack image gallery starter built with a Laravel API backend and a Vue 3 single-page frontend. It is structured for cookie-based SPA authentication with Laravel Sanctum and includes the core screens needed for an authenticated gallery experience: account registration, login, upload, and a personal image area.

The current codebase is a strong foundation for a private image management application. Authentication and layout are wired, while the upload and gallery pages are intentionally ready for the next phase: image storage, image metadata, gallery APIs, and image rendering.

## Project Overview

DashGallery is split into two applications:

- `backend/` - Laravel 11 API application with Sanctum, Breeze-style authentication controllers, database migrations, and PHPUnit feature tests.
- `frontend/` - Vue 3 and Vite SPA with Vue Router, Axios, Tailwind CSS, Headless UI navigation, and Heroicons.

The frontend communicates with the Laravel backend through a credentialed Axios client. Sanctum provides the CSRF cookie and browser-session authentication flow, making the project suitable for a first-party SPA where the frontend and backend are controlled together.

## Tech Stack

### Backend

- PHP 8.2+
- Laravel 11
- Laravel Sanctum
- Laravel Breeze auth controllers
- MySQL or any Laravel-supported database
- PHPUnit feature tests

### Frontend

- Vue 3
- Vite 6
- Vue Router 4
- Axios
- Tailwind CSS 4
- Headless UI Vue
- Heroicons

## Core Features

- User registration with name, email, password, and password confirmation.
- User login through Laravel's session-based authentication flow.
- CSRF protection using `GET /sanctum/csrf-cookie`.
- Credentialed frontend requests with Axios.
- Authenticated user endpoint at `GET /api/user`.
- Logout flow through `POST /logout`.
- Responsive application layout with desktop and mobile navigation.
- Upload and My Images routes prepared for gallery functionality.
- Laravel authentication tests included in the backend test suite.

## Current Application Flow

1. A visitor opens the Vue SPA.
2. The visitor can register at `/signup` or log in at `/login`.
3. Before submitting auth credentials, the frontend requests `/sanctum/csrf-cookie`.
4. Laravel creates an authenticated browser session after successful login or registration.
5. The user is redirected to the Upload screen.
6. The top navigation gives access to Upload and My Images.
7. The user can sign out from the profile menu.

## Project Structure

```text
DashGallery/
|-- README.md
|-- backend/
|   |-- app/
|   |   |-- Http/
|   |   |   |-- Controllers/Auth/     # Register, login, logout, reset, verification controllers
|   |   |   `-- Requests/Auth/        # Login request validation
|   |   `-- Models/User.php
|   |-- config/                      # Laravel, Sanctum, CORS, session, database config
|   |-- database/
|   |   |-- migrations/               # Users, cache, jobs, personal access tokens
|   |   |-- factories/
|   |   `-- seeders/
|   |-- routes/
|   |   |-- api.php                   # Authenticated API user route
|   |   |-- auth.php                  # SPA auth endpoints
|   |   `-- web.php
|   |-- tests/Feature/Auth/          # Authentication feature tests
|   |-- composer.json
|   `-- artisan
`-- frontend/
    |-- src/
    |   |-- components/
    |   |   |-- DefaultLayout.vue     # Authenticated app shell and navigation
    |   |   `-- GuestLayout.vue       # Login/register shell
    |   |-- pages/
    |   |   |-- Home.vue              # Upload route placeholder
    |   |   |-- MyImages.vue          # Personal gallery placeholder
    |   |   |-- Login.vue
    |   |   |-- Signup.vue
    |   |   `-- NotFound.vue
    |   |-- axios.js                  # API client with credentials and XSRF support
    |   |-- router.js                 # Vue Router routes
    |   |-- main.js
    |   `-- style.css
    |-- package.json
    `-- vite.config.js
```

## Frontend Routes

| Route | Component | Purpose |
| --- | --- | --- |
| `/` | `Home.vue` inside `DefaultLayout.vue` | Upload screen placeholder |
| `/images` | `MyImages.vue` inside `DefaultLayout.vue` | Personal image gallery placeholder |
| `/login` | `Login.vue` | Existing user login |
| `/signup` | `Signup.vue` | New user registration |
| `/:pathMatch(.*)*` | `NotFound.vue` | Fallback page |

## Backend Routes

### API

| Method | Endpoint | Middleware | Purpose |
| --- | --- | --- | --- |
| `GET` | `/api/user` | `auth:sanctum` | Return the authenticated user |

### Auth

| Method | Endpoint | Purpose |
| --- | --- | --- |
| `POST` | `/register` | Create a new user account |
| `POST` | `/login` | Authenticate an existing user |
| `POST` | `/logout` | End the authenticated session |
| `POST` | `/forgot-password` | Send a password reset link |
| `POST` | `/reset-password` | Reset a password |
| `GET` | `/verify-email/{id}/{hash}` | Verify an email address |
| `POST` | `/email/verification-notification` | Resend verification email |

## Requirements

- PHP 8.2 or newer
- Composer
- Node.js and npm
- MySQL, MariaDB, SQLite, PostgreSQL, or another Laravel-supported database

## Backend Setup

From the repository root:

```bash
cd backend
composer install
cp .env.example .env
php artisan key:generate
```

Update the database settings in `backend/.env`, then run:

```bash
php artisan migrate
php artisan serve
```

The backend usually runs at:

```text
http://localhost:8000
```

Recommended local SPA values for `backend/.env`:

```env
APP_URL=http://localhost:8000
FRONTEND_URL=http://localhost:5173
SANCTUM_STATEFUL_DOMAINS=localhost:5173,127.0.0.1:5173,localhost:8000,127.0.0.1:8000
SESSION_DOMAIN=localhost
```

## Frontend Setup

From the repository root:

```bash
cd frontend
npm install
npm run dev
```

The frontend usually runs at:

```text
http://localhost:5173
```

Create `frontend/.env` if it does not exist:

```env
VITE_API_BASE_URL=http://localhost:8000
```

## Running Both Apps

Use two terminals.

Terminal 1:

```bash
cd backend
php artisan serve
```

Terminal 2:

```bash
cd frontend
npm run dev
```

Open:

```text
http://localhost:5173
```

## Useful Commands

Backend:

```bash
composer install
php artisan migrate
php artisan test
php artisan route:list
php artisan serve
```

Frontend:

```bash
npm install
npm run dev
npm run build
npm run preview
```

## Implementation Notes

- `frontend/src/axios.js` sets `withCredentials: true` and `withXSRFToken: true`, which is required for Sanctum SPA authentication.
- Login and registration request the CSRF cookie before posting credentials.
- `DefaultLayout.vue` contains the authenticated navigation shell and logout action.
- `Home.vue` and `MyImages.vue` currently define the page shells but do not yet upload, store, or render images.
- `backend/routes/api.php` currently exposes only the authenticated user endpoint.
- The backend already includes Laravel's default auth-related feature tests.

## Known Gaps

- The upload page does not yet include a file picker, preview, validation, or upload request.
- The backend does not yet include image models, migrations, storage logic, or image API routes.
- The My Images page does not yet fetch or render uploaded images.
- The frontend currently uses placeholder profile data in the authenticated layout.
- Auth forms need user-facing validation and error display cleanup.

## Suggested Next Development Steps

1. Add an `images` database table with owner, title, filename, path, MIME type, size, and timestamps.
2. Create authenticated API routes for image upload, image listing, image download/viewing, and image deletion.
3. Store images through Laravel's filesystem abstraction, preferably using the `public` disk for local development.
4. Build the upload form with drag-and-drop, progress state, file preview, and validation feedback.
5. Build the My Images grid with loading, empty, error, and delete states.
6. Replace placeholder user data in `DefaultLayout.vue` with data from `GET /api/user`.
7. Add tests for image upload authorization, validation, listing, and deletion.

## Image Generation Prompts

Use these prompts to create README, portfolio, or presentation images for this project.

### 1. Frontend UI Prompt

Create a high-resolution product screenshot style image for "DashGallery", a modern Vue 3 image gallery web app. Show a clean authenticated dashboard in a desktop browser window with a dark top navigation bar, an Upload tab, a My Images tab, a profile dropdown, and a polished upload workspace. The main screen should include a large drag-and-drop image upload area, thumbnail previews, file names, progress indicators, and a simple gallery grid below it. Use a professional SaaS interface style with Tailwind CSS inspired spacing, white and light gray surfaces, indigo accents, crisp typography, subtle shadows, and responsive web app details. The image should look like a real frontend product UI, not a marketing landing page. No random unreadable text, no fake code, no distorted buttons. Aspect ratio 16:9, sharp, clean, realistic browser chrome.

### 2. Backend Architecture Prompt

Create a detailed technical architecture illustration for the backend of "DashGallery", a Laravel 11 API powered by Laravel Sanctum. Visualize a Vue SPA on the left sending authenticated Axios requests to a Laravel API server in the center. Show the CSRF cookie flow, login/register/logout endpoints, protected `/api/user` route, Sanctum session authentication, controllers, middleware, database migrations, and a future image storage layer connected to a database and filesystem disk. Use clean diagram styling with labeled boxes, directional arrows, subtle Laravel red accents, neutral background, and clear separation between browser, API, authentication, database, and file storage. Make it suitable for a GitHub README architecture section. Aspect ratio 16:9, professional software architecture diagram, readable labels, minimal clutter.

### 3. Visual Brand Prompt

Create a polished hero/banner visual for "DashGallery", a private full-stack image gallery application. Show a modern laptop or wide monitor displaying an elegant personal image gallery with upload controls, photo thumbnails, secure account navigation, and a clean dashboard layout. Add subtle visual cues for full-stack development: a small Laravel API panel, Vue component cards, secure session/authentication symbols, and image storage elements in the background. The mood should feel reliable, creative, and developer-friendly. Use balanced colors: white, graphite, indigo, soft green, and small Laravel red highlights. Avoid dark blurry stock-photo style; keep the product screen clear and inspectable. No people required, no exaggerated 3D icons, no fantasy elements. Aspect ratio 16:9, high detail, GitHub README banner quality.

## License

This project uses the Laravel application skeleton license information from `backend/composer.json`.
