# DashGallery

DashGallery is a full-stack private image gallery project built with a Laravel API backend and a Vue 3 single-page frontend. The application is structured around first-party SPA authentication with Laravel Sanctum, so the browser client can register users, log users in, keep an authenticated session, and call protected API routes with CSRF protection.

The repository currently provides the foundation for an authenticated gallery product: account registration, login, logout, protected navigation, an Upload route, and a My Images route. The image upload, storage, listing, and deletion features are prepared as the next development phase.

## Project Overview

This repository contains two separate applications:

| App | Path | Purpose |
| --- | --- | --- |
| Backend | `backend/` | Laravel 11 API, Sanctum authentication, Breeze-style auth controllers, database migrations, and PHPUnit tests |
| Frontend | `frontend/` | Vue 3 + Vite SPA with Vue Router, Axios, Tailwind CSS, Headless UI, and Heroicons |

The frontend uses a credentialed Axios client to communicate with Laravel. Before login or registration, it requests the Sanctum CSRF cookie from `/sanctum/csrf-cookie`, then submits credentials to the Laravel auth endpoints. Laravel creates a normal browser session, and protected API routes can then be accessed through Sanctum's `auth:sanctum` middleware.

## Tech Stack

### Backend

- PHP 8.2+
- Laravel 11
- Laravel Sanctum 4
- Laravel Breeze authentication controllers
- Eloquent ORM and Laravel migrations
- PHPUnit 11 feature and unit tests
- Composer for dependency management

### Frontend

- Vue 3
- Vite 6
- Vue Router 4
- Axios
- Tailwind CSS 4
- Headless UI Vue
- Heroicons
- npm for dependency management

## Core Features

- User registration with name, email, password, and password confirmation.
- User login through Laravel session authentication.
- CSRF-protected SPA auth flow using Laravel Sanctum.
- Authenticated `GET /api/user` endpoint.
- Logout through `POST /logout`.
- Vue Router based pages for Upload, My Images, Login, Signup, and Not Found.
- Authenticated layout with desktop and mobile navigation.
- Guest layout for authentication screens.
- Axios client configured for credentials and XSRF support.
- Backend auth feature tests included.

## Current Application Flow

1. A visitor opens the Vue SPA.
2. The visitor registers at `/signup` or logs in at `/login`.
3. The frontend calls `GET /sanctum/csrf-cookie`.
4. The frontend posts credentials to `/register` or `/login`.
5. Laravel validates the request, creates or authenticates the user, regenerates the session, and returns an empty success response.
6. The frontend redirects the user to the Upload screen.
7. The authenticated layout gives access to Upload, My Images, and Sign out.
8. Sign out posts to `/logout`, invalidates the Laravel session, and returns the user to Login.

## Project Structure

```text
DashGallery/
|-- README.md
|-- backend/
|   |-- app/
|   |   |-- Http/
|   |   |   |-- Controllers/Auth/      # Register, login, logout, password, and verification controllers
|   |   |   |-- Middleware/            # Email verification middleware
|   |   |   `-- Requests/Auth/         # Login request validation
|   |   |-- Models/User.php
|   |   `-- Providers/
|   |-- config/                       # Laravel, Sanctum, CORS, session, and database config
|   |-- database/
|   |   |-- factories/UserFactory.php
|   |   |-- migrations/                # Users, cache, jobs, personal access tokens
|   |   `-- seeders/DatabaseSeeder.php
|   |-- routes/
|   |   |-- api.php                    # Protected API routes
|   |   |-- auth.php                   # SPA auth endpoints
|   |   |-- console.php
|   |   `-- web.php
|   |-- tests/
|   |   |-- Feature/Auth/              # Authentication test coverage
|   |   |-- Feature/ExampleTest.php
|   |   `-- Unit/ExampleTest.php
|   |-- composer.json
|   `-- artisan
`-- frontend/
    |-- src/
    |   |-- components/
    |   |   |-- DefaultLayout.vue      # Authenticated shell and navigation
    |   |   `-- GuestLayout.vue        # Login/register wrapper
    |   |-- pages/
    |   |   |-- Home.vue               # Upload page shell
    |   |   |-- MyImages.vue           # Personal gallery page shell
    |   |   |-- Login.vue
    |   |   |-- Signup.vue
    |   |   `-- NotFound.vue
    |   |-- axios.js                   # Credentialed API client
    |   |-- router.js                  # Vue Router configuration
    |   |-- main.js
    |   `-- style.css
    |-- package.json
    `-- vite.config.js
```

## Frontend Routes

| Route | Component | Layout | Purpose |
| --- | --- | --- | --- |
| `/` | `Home.vue` | `DefaultLayout.vue` | Upload page shell |
| `/images` | `MyImages.vue` | `DefaultLayout.vue` | Personal image gallery page shell |
| `/login` | `Login.vue` | `GuestLayout.vue` | Existing user login |
| `/signup` | `Signup.vue` | `GuestLayout.vue` | New user registration |
| `/:pathMatch(.*)*` | `NotFound.vue` | None | Fallback page |

## Backend Routes

### API Routes

| Method | Endpoint | Middleware | Purpose |
| --- | --- | --- | --- |
| `GET` | `/api/user` | `auth:sanctum` | Return the authenticated user |

### Auth Routes

| Method | Endpoint | Middleware | Purpose |
| --- | --- | --- | --- |
| `POST` | `/register` | `guest` | Create a new account and log the user in |
| `POST` | `/login` | `guest` | Authenticate an existing user |
| `POST` | `/logout` | `auth` | End the authenticated session |
| `POST` | `/forgot-password` | `guest` | Send a password reset link |
| `POST` | `/reset-password` | `guest` | Reset a password |
| `GET` | `/verify-email/{id}/{hash}` | `auth`, `signed`, `throttle` | Verify an email address |
| `POST` | `/email/verification-notification` | `auth`, `throttle` | Resend verification email |

## Requirements

- PHP 8.2 or newer
- Composer
- Node.js and npm
- MySQL, MariaDB, PostgreSQL, SQLite, or another Laravel-supported database

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

Open the frontend in the browser:

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
- Login and registration both request `/sanctum/csrf-cookie` before posting credentials.
- `DefaultLayout.vue` contains the authenticated navigation shell, mobile menu, profile dropdown, and logout action.
- `Home.vue` and `MyImages.vue` are page shells for the gallery workflow.
- `backend/routes/api.php` currently exposes the authenticated user endpoint.
- `backend/routes/auth.php` exposes Breeze-style auth endpoints for registration, login, logout, password reset, and email verification.
- Backend authentication tests are available under `backend/tests/Feature/Auth`.

## Current Status

The authentication foundation is in place. The app can support the normal Sanctum SPA login lifecycle once the backend environment and database are configured. The gallery-specific domain is the next missing layer: image database records, file storage, upload validation, authenticated image APIs, and frontend image rendering.

## Known Gaps

- The Upload page does not yet include a file picker, drag-and-drop area, preview, validation, progress state, or upload request.
- The backend does not yet include image models, image migrations, storage logic, or image API routes.
- The My Images page does not yet fetch or render uploaded images.
- The frontend currently uses placeholder profile data in `DefaultLayout.vue` instead of loading `GET /api/user`.
- The auth forms log validation errors to the console but do not yet show polished user-facing error messages.
- The login page heading currently says "Create an account", which should be adjusted to "Sign in" during UI cleanup.

## Suggested Next Development Steps

1. Add an `images` database table with `user_id`, title, original filename, stored filename, path, MIME type, size, width, height, and timestamps.
2. Create an `Image` Eloquent model with a relationship to `User`.
3. Add authenticated API routes for image upload, image listing, image viewing or download, and image deletion.
4. Store image files through Laravel's filesystem abstraction, using the `public` disk for local development.
5. Run `php artisan storage:link` so public gallery files can be served locally.
6. Build the Upload UI with file selection, drag-and-drop, preview thumbnails, upload progress, loading state, and validation feedback.
7. Build the My Images grid with loading, empty, error, pagination, preview, and delete states.
8. Replace placeholder user data in `DefaultLayout.vue` with data from `GET /api/user`.
9. Add backend tests for image upload authorization, validation, listing, ownership, and deletion.
10. Add frontend build checks before deployment.

## Image Generation Prompts

Use these prompts to create project images for the GitHub README, portfolio, presentation slides, or social preview graphics.

### 1. Frontend UI Prompt

Create a high-resolution product screenshot style image for "DashGallery", a modern Vue 3 private image gallery web application. Show the authenticated frontend running inside a realistic desktop browser window. The top navigation should have a dark header, the DashGallery logo area, an active Upload tab, a My Images tab, a notification icon, and a profile dropdown. The main Upload screen should feel like a real SaaS dashboard: a clean page header, a large drag-and-drop image upload area, selected image thumbnails, file names, MIME type and size metadata, upload progress bars, validation states, and a compact recent uploads gallery grid below. Use Tailwind CSS inspired spacing, white and light gray surfaces, crisp typography, indigo action accents, subtle borders, and restrained shadows. Include responsive design hints such as a mobile menu icon in the browser preview or a small secondary mobile mockup beside the desktop view. Make the interface practical and inspectable, not a marketing landing page. Avoid random unreadable placeholder text, distorted buttons, fake code blocks, excessive gradients, dark blurry backgrounds, or decorative shapes. Aspect ratio 16:9, sharp details, professional GitHub README quality.

### 2. Backend Architecture Prompt

Create a detailed technical architecture diagram for the backend of "DashGallery", a Laravel 11 API secured with Laravel Sanctum for a first-party Vue SPA. Place the Vue/Vite frontend browser on the left, the Laravel API server in the center, and persistence services on the right. Show the browser requesting `GET /sanctum/csrf-cookie`, then sending credentialed Axios requests to `POST /register`, `POST /login`, `POST /logout`, and protected `GET /api/user`. In the Laravel server area, include labeled modules for routes, auth controllers, login request validation, Sanctum middleware, session guard, CSRF protection, Eloquent User model, migrations, and PHPUnit tests. Add a clearly marked future gallery layer with Image model, upload controller, validation, storage service, database records, and filesystem/public disk. Use clean directional arrows, readable labels, neutral background, Laravel red highlights, Vue green highlights, and database/storage icons. The diagram should be technically accurate, polished, and suitable for a GitHub README architecture section. Aspect ratio 16:9, minimal clutter, no fake source code, no tiny unreadable text.

### 3. Visual Brand Prompt

Create a polished hero/banner visual for "DashGallery", a full-stack private image gallery application. Show a modern laptop or wide monitor with the actual product concept visible: an authenticated dashboard, upload controls, image thumbnail grid, secure profile navigation, and clean gallery management actions. Around the main screen, add subtle visual cues for the stack and workflow: Vue component cards, Laravel API endpoint panels, Sanctum/session security badges, database rows, and image storage folders connected by fine lines. The mood should feel reliable, creative, developer-friendly, and production-oriented. Use a balanced color palette of white, graphite, indigo, soft green, and small Laravel red highlights. Keep the product screen bright, clear, and inspectable. Avoid people, fantasy elements, exaggerated 3D mascots, stock-photo blur, dark dramatic lighting, random text, or unreadable UI. Design it as a GitHub README banner or portfolio cover image with strong composition and generous spacing. Aspect ratio 16:9, high detail, clean software-product aesthetic.

## License

This project uses the Laravel application skeleton license information from `backend/composer.json`.
