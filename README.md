# AuthFrontend

This project was generated using [Angular CLI](https://github.com/angular/angular-cli) version 22.3.0.

## Development server

To start a local development server, run:

```bash
ng serve
```

Once the server is running, open your browser and navigate to `http://localhost:4200/`. The application will automatically reload whenever you modify any of the source files.

## Code scaffolding

Angular CLI includes powerful code scaffolding tools. To generate a new component, run:

```bash
ng generate component component-name
```

For a complete list of available schematics (such as `components`, `directives`, or `pipes`), run:

```bash
ng generate --help
```

## Building

To build the project run:

```bash
ng build
```

This will compile your project and store the build artifacts in the `dist/` directory. By default, the production build optimizes your application for performance and speed.

## Running unit tests

To execute unit tests with the [Vitest](https://vitest.dev/) test runner, use the following command:

```bash
ng test
```

## Running end-to-end tests

For end-to-end (e2e) testing, run:

```bash
ng e2e
```

Angular CLI does not come with an end-to-end testing framework by default. You can choose one that suits your needs.

## Additional Resources

For more information on using the Angular CLI, including detailed command references, visit the [Angular CLI Overview and Command Reference](https://angular.dev/tools/cli) page.
Auth Frontend (Angular 21)

A modern authentication frontend built with Angular 21, connected to a Node.js + TypeScript backend (the professional-boilerplate API). It covers the full user account lifecycle: registration, email verification, login with JWT, a protected dashboard, and a forgot/reset password flow.

Project Description

This project is the Angular client for the professional-boilerplate REST API. Users can create an account, confirm their email address through a verification link, sign in, view their profile on a protected dashboard, and recover access through a password reset email.

It is built with standalone components, signals, functional route guards, and HTTP interceptors, and it demonstrates how a frontend, a backend API, a database, and an email service work together as one integrated application.

Features
User registration with form validation (name, email, password rules)
Email verification through a link sent to the user's inbox
Login with JWT authentication, with the session kept across page refreshes
Protected dashboard that shows the signed-in user's account details
Forgot password and reset password using a time-limited emailed token (expires in 1 hour)
Route guards: authGuard protects private pages, guestGuard keeps signed-in users away from login and register
HTTP interceptors: an auth interceptor attaches the JWT to requests, and an error interceptor handles API errors in one place
Lazy-loaded routes for a smaller initial bundle
Reactive state with signals (signal, computed) in a single AuthService
Reactive forms with client-side validation and clear error messages
Logout that clears the session and returns the user to login
Tech Stack
Layer	Technology
Frontend	Angular 21, TypeScript, RxJS, Reactive Forms
Backend	Node.js, Express, TypeScript (ts-node-dev)
Auth	JWT, bcrypt password hashing
Email	Nodemailer with Ethereal (test inbox)
Project Structure
auth-frontend/
├── src/
│   ├── app/
│   │   ├── core/
│   │   │   ├── guards/          # auth.guard.ts, guest.guard.ts
│   │   │   ├── interceptors/    # auth.interceptor.ts, error.interceptor.ts
│   │   │   └── services/        # auth.service.ts (single source of truth)
│   │   ├── features/
│   │   │   ├── auth/            # login, register, forgot-password,
│   │   │   │                    # reset-password, verify-email
│   │   │   └── dashboard/
│   │   ├── app.config.ts
│   │   ├── app.routes.ts
│   │   └── app.ts
│   └── environments/
├── docs/screenshots/            # screenshots used in this README
└── README.md
Setup Instructions
Prerequisites
Node.js (current LTS version) and npm
Angular CLI: npm install -g @angular/cli
The professional-boilerplate backend (see step 1)
1. Start the backend
powershell
cd "professional-boilerplate 2\professional-boilerplate"
npm install
npm run dev

Make sure the backend .env is configured (database connection, JWT secret, email settings, and FRONTEND_URL=http://localhost:4200). When it starts correctly you should see:

Database connected successfully
Server running on port 3000
Email service connected
2. Configure the frontend

Open src/environments/environment.ts and make sure apiUrl points to your backend, for example:

typescript
export const environment = {
  production: false,
  apiUrl: 'http://localhost:3000/api'
};

Adjust the value if your backend mounts its routes on a different path.

3. Start the frontend

Open a second terminal (keep the backend running):

powershell
cd auth-frontend
npm install
ng serve

Then open http://localhost:4200 in your browser.

4. Try the full flow
Register a new account at /auth/register
Open the verification email (Ethereal inbox or backend console) and click the link
Log in at /auth/login
View your details on the dashboard
Test Forgot password, click the emailed reset link, set a new password, and log in again
Troubleshooting
Problem	Fix
Port 4200 is already in use	Stop the other ng serve (Ctrl + C) before starting a new one
Blank page	Open the browser console (F12) and check for errors; confirm app.html contains only <router-outlet />
Could not resolve "@angular/animations/browser"	Run npm install @angular/animations
NG0908 (requires Zone.js)	Remove provideZoneChangeDetection from app.config.ts
API calls fail	Make sure the backend is running on port 3000 and apiUrl is correct
Routes
Route	Description	Guard
/auth/login	Login form	guestGuard
/auth/register	Registration form	guestGuard
/auth/forgot-password	Request a reset link by email	guestGuard
/auth/reset-password?token=...	Set a new password	guestGuard
/verify-email?token=...	Verify email address	none
/dashboard	Protected user dashboard	authGuard
Screenshots

Save your screenshots in docs/screenshots/ using the file names below and they will appear here.

Register

Register page

Email Verification

Verify email page

Login

Login page

Dashboard

Dashboard page

Forgot Password

Forgot password page

Password Reset Email

Password reset email

Reset Password

Reset password page

Reflection Questions
1. What are the three main modern Angular features you used in this project? How do they improve the developer experience?
Standalone components. Each component declares its own imports array, so there are no NgModule files to maintain. Dependencies are visible right next to the component that uses them, which makes the code easier to read, move, and lazy load.
Signals (signal, computed). The AuthService keeps its state (current user, token, loading, error) in signals. The UI updates automatically when a signal changes, without manual subscriptions or ngOnDestroy cleanup.
Functional APIs (inject(), functional guards, functional interceptors). Guards and interceptors are plain functions (CanActivateFn, withInterceptors([...])) and dependencies come from inject(). This means less boilerplate than class-based versions and the code is easier to test.

Together they reduce boilerplate, make data flow easier to follow, and catch more mistakes at compile time.

2. Explain the difference between Signals and RxJS. When would you use each?

Signals hold a current value. They are synchronous, always have a value, and automatically notify anything that reads them (templates, computed, effect) when the value changes. RxJS Observables represent a stream of values over time. They can be asynchronous, can complete or error, and come with powerful operators such as map, switchMap, debounceTime, and retry.

Use Signals for UI and application state: currentUser, isLoggedIn, isLoading, form status, and derived values with computed.
Use RxJS for asynchronous events and streams: HTTP requests (HttpClient returns Observables), search-as-you-type, combining multiple async sources, and retry or cancellation logic.

In this project the two work together. HttpClient returns Observables for API calls (login(), register()), and when a call succeeds, the result is stored in signals that the templates read.

3. Why is it important to have both an auth interceptor and an error interceptor?

They solve two different problems, and both are cross-cutting concerns that would otherwise be repeated in every service.

The auth interceptor runs on every outgoing request and attaches the JWT (Authorization: Bearer <token>) so protected endpoints work without each service adding the header manually.
The error interceptor runs on every response and handles failures in one place, such as an expired or invalid token (401) or server errors. This gives consistent behavior, for example logging the user out and redirecting to login when the token is no longer valid.

Keeping them separate follows single responsibility, avoids duplicated code, and makes both easier to change or test.

4. How do route guards improve security and user experience?
authGuard blocks /dashboard for users who are not logged in and redirects them to /auth/login, remembering the page they wanted through the returnUrl query parameter.
guestGuard keeps logged-in users away from login and register pages by redirecting them to the dashboard.

For user experience, people never see a page they cannot use, and they land in the right place after logging in. For security, guards stop unauthorized users from reaching protected views on the client. However, guards are only a UI convenience. The real security is enforced by the backend, which validates the JWT on every protected API request. A user can bypass frontend checks, but they cannot get data without a valid token.

5. What is lazy loading and why is it important for performance?

Lazy loading means a route's code is downloaded only when the user navigates to it, instead of being bundled into the first download. This project uses loadComponent: () => import(...) for every page (login, register, forgot password, reset password, verify email, dashboard).

This keeps the initial bundle small, so the first load is faster, especially on slow networks. Users who only visit the login page never download the dashboard code. It also scales well as the application grows.

6. Explain the "single source of truth" principle in the context of your AuthService.

All authentication state lives in one place: the AuthService. The token, current user, loading state, and error message are stored in private signals inside the service and exposed to the rest of the app as read-only computed signals (isLoggedIn, currentUser, isLoading, error).

Components never keep their own copy of "who is logged in". The guards, interceptors, dashboard, and forms all read from the same service. When the service updates (for example on login or logout), every consumer updates automatically. This prevents the state from getting out of sync between components, and any bug in auth logic only has one place to fix. The service also syncs this state to localStorage so the session survives a page refresh.

7. What challenges did you face when connecting the Angular frontend to the Node.js backend? How did you solve them?
Running two servers at once. The backend (port 3000) and frontend (port 4200) must both run in separate terminals. I confirmed the backend was up ("Database connected", "Server running") before testing the frontend.
Wrong folder when running commands. Running cd or npm run dev in the wrong folder caused "path does not exist" and "package.json not found" errors. I solved it by checking the current directory in the prompt and using the correct inner project folder.
Build errors in the frontend. A misspelled stylesheet filename (register.components.css instead of register.component.css) and a missing DatePipe import in the dashboard both stopped compilation. I fixed them by renaming the file and importing DatePipe.
Blank page on startup. The default Angular placeholder in app.html had to be replaced with only <router-outlet />. Then the console showed two runtime errors: a missing @angular/animations package (fixed with npm install @angular/animations) and NG0908 about Zone.js (fixed by removing provideZoneChangeDetection from app.config.ts, since the app uses signals).
Port already in use. A leftover ng serve was still running on port 4200. I stopped that process before starting a new one.
Reset password link going to the wrong page. The email link pointed to /reset-password, but the route is /auth/reset-password, so the wildcard route redirected the user away. I fixed the link in the backend email service and added a redirect route in the frontend.
8. If you had to add a new feature (e.g., user profile editing), how would you approach it using the patterns you learned?
Backend: add a protected PUT /auth/me (or /users/me) endpoint with validation, and return the updated user.
Model: add an UpdateProfileRequest interface that matches the backend DTO.
Service: add an updateProfile() method to AuthService. On success, update the currentUserSignal and localStorage, so the single source of truth stays accurate everywhere.
Component: create a standalone ProfileComponent with a reactive form (FormBuilder and validators), reusing the loading and error signals.
Routing: add a lazy-loaded /profile route protected by authGuard, plus a link from the dashboard.

The existing auth interceptor already attaches the token, and the error interceptor already handles failures, so neither needs changes.

9. What security considerations did you implement in the frontend (token storage, XSS prevention, etc.)?
Token handling: the JWT is sent in the Authorization header by the auth interceptor, and it is cleared from state and storage on logout.
Token storage trade-off: the token is stored in localStorage so sessions survive refresh. This is simple, but it is readable by JavaScript, so an XSS bug could expose it. A more secure production option is an HttpOnly, Secure, SameSite cookie set by the backend.
XSS prevention: Angular escapes interpolated values ({{ }}) by default, and I avoided innerHTML and bypassSecurityTrust*, so user input is not rendered as HTML.
Route guards: unauthenticated users cannot open protected pages (with the backend as the real enforcement).
Form validation: reactive forms enforce required fields, email format, and password rules (minimum 8 characters with letters and numbers) before submitting. The backend validates again, since client-side validation can be bypassed.
Password handling: passwords are only sent over the API to be hashed with bcrypt by the backend; the frontend never stores them. Password reset tokens are single-use links that expire after 1 hour.
Safe error messages: the UI shows friendly messages from the API rather than raw errors.
10. How does this Angular frontend demonstrate the "Integrative Programming" concepts from this course?

Integrative Programming is about making separate systems and technologies work together as one application. This project integrates:

A frontend and a backend that communicate through a REST API using JSON, with TypeScript interfaces (User, AuthResponse, ApiResponse<T>) that mirror the backend DTOs.
Authentication across layers: the backend issues a JWT, and the frontend stores it, attaches it to requests with an interceptor, and uses it to guard routes.
An external email service (Nodemailer with Ethereal) that sends verification and password reset emails containing links back into the Angular app.
A database behind the API that persists users, with the frontend reflecting that data on the dashboard.
Environment configuration (environment.apiUrl, backend frontendUrl) so each part knows where the others live.
Cross-cutting concerns (interceptors, guards, shared services) that keep the integrated pieces consistent.

Getting the full flow to work end to end (register, verify email, login, dashboard, forgot password, reset password) required all of these parts to agree on URLs, data formats, and tokens.
