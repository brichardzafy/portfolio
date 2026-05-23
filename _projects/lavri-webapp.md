---
layout: page
title: Lavri Webapp
description: Next.js 16 customer webapp for the Lavri connected laundry network
img: assets/img/laverie.png
importance: 3
category: work
giscus_comments: false
---

**Lavri Webapp** is the customer-facing web application of the Lavri connected
laundry network. End-users log in with their phone number and PIN code, top up
their balance, book a washer or dryer, and manage their account from a single,
mobile-first interface. On this project I work as a **Front-End & Mobile
developer**: I build the screens, the auth flows and the entire client layer
that talks to the Lavri backend.

## Tech stack

| Technology                | Role                                              |
| ------------------------- | ------------------------------------------------- |
| **Next.js 16** (App Router) | Fullstack framework                             |
| **React 19**              | UI layer                                          |
| **TypeScript**            | Static typing                                     |
| **Tailwind CSS v4**       | Utility-first styling                             |
| **NextAuth v4**           | Sessions (JWT + Credentials, Google, Apple)       |
| **Zustand v5**            | Client-side global state (persisted in `sessionStorage`) |
| **React Hook Form**       | Form management                                   |
| **Lucide React**          | Icon set                                          |

## Main features

- **Authentication** — phone + PIN sign-in via NextAuth Credentials, with
  Google and Apple OAuth providers as alternatives.
- **Account creation** — full sign-up form (name, phone, PIN, gender…) calling
  a dedicated subscription API route.
- **PIN reset (3-step OTP flow)** — request OTP by SMS → verify 6-digit code →
  set new PIN, with intermediate state held in the Zustand store and route
  guards on each step.
- **User space (`/mon-espace`)** — balance, top-up, attendance indicator,
  washers and dryers availability, reservation, history and settings.
- **Splash screen** — branded boot screen that routes to login or to the user
  space depending on session state.
- **Session sync** — an `AuthSync` component bridges the NextAuth session into
  the Zustand store on every page load.

## Front-end architecture

- **Next.js App Router** under `app/` — public auth pages (`/auth/...`),
  splash screen, and the authenticated `mon-espace/[...slug]` area with
  dynamic dashboard routes.
- **API routes** — `api/auth/[...nextauth]` (NextAuth handler),
  `api/auth/subscription` (account creation), `api/auth/new-pin` (PIN update).
- **NextAuth configuration** (`lib/auth.ts`) — JWT strategy with a custom
  Credentials provider calling the Lavri backend (`/app-user-login`).
- **Backend client** — a typed `BEDApiCore` base class plus a `UserApi` service
  (`lib/api/backend/`) centralizing login, sign-up, avatar and PIN endpoints.
- **State management** —
  - `userStore` (auth slice): `user`, `isAuthenticated`, `stepVerification`,
    `errors`, plus actions `signInByPhone`, `signOut`, `resetPinCode`,
    `setNewPin`, `setStepVerification`, `getDefaultHomePage`.
  - `toastStore` (notifications slice) driving a reusable `Toast` component.
  - Both stores are persisted in `sessionStorage` (key `lavri-store-user`) so
    they survive reloads but clear on tab close.
- **Layouts** — `AuthLayout` (centered white card for auth pages) and
  `AppLayout` (sidebar + content for the authenticated area).
- **API helpers** — `apiNextEndPoint.ts` centralizes API URLs and
  `apiResponse.ts` provides standardized `apiSuccess()` / `apiError()` helpers
  for the Next.js route handlers.

## PIN reset flow

```
/auth/reinitialisation-pin
        │  resetPinCode() → backend sends 6-digit OTP by SMS
        ▼
/auth/verification-pin
        │  6-digit OTP entry, compared to the stored code
        ▼
/auth/nouveau-pin
        │  setNewPin() → backend resetPasswordByToken({ token, code_pin })
        ▼
/auth/login
```

Each step is route-guarded against the `isStepVerification` flag in the store,
and the verification state is cleared as soon as the new PIN is accepted.

## My role

As a **Front-End & Mobile developer** on Lavri Webapp, I cover:

- The full UI — branded splash screen, auth pages, dashboard layout and
  reservation screens — implemented with Next.js 16, React 19 and
  Tailwind v4 in a mobile-first approach.
- The NextAuth configuration (Credentials + Google + Apple) and the secure
  JWT-based session lifecycle.
- The multi-step PIN reset flow, including OTP UX, route guards and store
  modeling.
- The Zustand stores (`userStore`, `toastStore`) and the `AuthSync` bridge
  with NextAuth.
- The typed API client layer (`BEDApiCore`, `UserApi`) and the standardized
  API response helpers used by the Next.js route handlers.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/laverie-webapp/login.png" title="Lavri Webapp — login" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/laverie-webapp/dashboard.png" title="Lavri Webapp — user home" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Lavri Webapp — user home and login screens.
</div>
