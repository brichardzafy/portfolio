---
layout: page
title: Event Zaratof
description: Personal event-photo platform with face-recognition-powered photo retrieval. Built from scratch as a fullstack solo project.
img: assets/img/event-zaratof.png
importance: 1
category: own
giscus_comments: false
---

[Event Zaratof](https://www.event-zaratof.com/) is a personal project I built end-to-end: a web platform where
event organizers can publish their event galleries, and attendees can find every photo they appear in by simply
uploading a selfie. Behind the scenes, a Python service powered by
[`ageitgey/face_recognition`](https://github.com/ageitgey/face_recognition) matches the selfie against every face
detected in the event's photo set and returns the matching shots.

It's a fullstack solo build — frontend, REST API, OAuth2 authorization, database, face-recognition microservice
and deployment — and it lives in the **Own invention** section of this portfolio because the product, the design
and the architecture are all mine.

## Stack

| Layer | Tech |
| --- | --- |
| Frontend | React (Create React App), bootstrapped with `yarn`, Node 18.19.0 via `nvm` (built on macOS Monterey 12.7.2) |
| Backend API | PHP / Symfony 4, Doctrine ORM with migrations |
| Auth | OAuth2 — `FOSOAuthServerBundle`, `authorization_code` grant |
| Database | MySQL |
| Face recognition | Python service built on [`ageitgey/face_recognition`](https://github.com/ageitgey/face_recognition) (dlib-based) |
| Web server | Nginx / Apache, `public/` document root, URL rewriting to `index.php` |

## What it does

- **Event galleries** — organizers create an event and upload the full photo set.
- **Selfie-based search** — an attendee uploads a single selfie; the face-recognition service computes a face
  embedding and returns every photo of the gallery where that face appears.
- **OAuth2-secured API** — the Symfony backend issues access tokens via the `authorization_code` grant
  (`FOSOAuthServerBundle`) so the React frontend (and any future mobile client) talks to the API as a registered
  OAuth client.
- **Production-grade Symfony deploy** — `composer install --no-dev --optimize-autoloader`, Doctrine migrations
  (`doctrine:migrations:migrate`), prod cache warm-up (`cache:clear` / `cache:warmup --env=prod`), `.env.local` for
  secrets (`APP_ENV=prod`, `APP_SECRET`, `DATABASE_URL`).

## Frontend (Create React App)

Standard CRA workflow with `yarn`:

```bash
yarn start   # dev server on http://localhost:3000
yarn test    # interactive test runner
yarn build   # optimized production bundle in build/
```

Local dev environment: macOS Monterey 12.7.2, Node `v18.19.0` managed via `nvm`.

## Backend (Symfony 4)

Production deployment checklist used for the API:

```bash
# 1. Install dependencies (production-optimized)
composer install --no-dev --optimize-autoloader

# 2. .env.local (not committed)
#    APP_ENV=prod
#    APP_SECRET=...
#    DATABASE_URL="mysql://user:pass@127.0.0.1:3306/db"

# 3. Database
php bin/console doctrine:migrations:migrate --no-interaction

# 4. Cache
php bin/console cache:clear   --env=prod
php bin/console cache:warmup  --env=prod
```

Web server (Nginx or Apache) points the document root at `public/` and rewrites all requests to `index.php`.

## OAuth2 — authorization code

The API is secured by `FOSOAuthServerBundle`. An OAuth client is created via the Symfony console:

```bash
php bin/console fos:oauth-server:create-client \
    --redirect-uri="http://localhost:8000/redirect-uri-example" \
    --grant-type="authorization_code"
```

The bundle then returns a numeric `id`, a `random_id` and a `secret`. The effective `client_id` exposed to OAuth
clients is `{id}_{random_id}`, paired with the generated `secret`.

## Face-recognition service

Photo matching relies on [`ageitgey/face_recognition`](https://github.com/ageitgey/face_recognition), a Python
library built on top of dlib's state-of-the-art face-recognition model. Each uploaded event photo is processed to
extract face encodings; at search time, the attendee's selfie is encoded and compared against the stored
encodings, returning every match above a similarity threshold.

## My role

Everything: product design, UX, React frontend, Symfony 4 API, MySQL schema and Doctrine migrations, OAuth2
authorization-code flow, Python face-recognition microservice, server provisioning and deployment at
[event-zaratof.com](https://www.event-zaratof.com/).
