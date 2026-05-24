---
layout: page
title: Selima
description: Fullstack solo project — a Symfony 7.3 backend (admin back-office + REST API + public front) paired with an Expo / React Native mobile app.
img: assets/img/selima.png
importance: 2
category: own
giscus_comments: false
---

**Selima** is a personal fullstack project I'm building end-to-end: a modular **Symfony 7.3** backend
that powers an admin back-office, a documented REST API and a public front-end, together with a
cross-platform **Expo / React Native** mobile app that consumes the API.

Like the rest of the **Own invention** section, the product, the architecture and every line of code
are mine — backend, mobile, infra and deploy.

## Stack

| Layer | Tech |
| --- | --- |
| Backend | PHP 8.2+ / **Symfony 7.3**, custom bundles (Admin / Api / Front / MetierManager) |
| ORM / DB | Doctrine ORM, **MySQL 8.0** |
| API | FOS REST Bundle, JMS Serializer, **Nelmio API Doc** (Stoplight UI) |
| Front (web) | Symfony AssetMapper + Stimulus + UX Turbo |
| Mobile | **Expo** / React Native, file-based routing via `expo-router` |
| Auth | Session-based form login (admin) + stateless token auth (API) |
| Infra | Docker / Docker Compose, Apache, phpMyAdmin |

## Backend — Symfony 7.3

The app is split into four custom bundles under `src/Selima/`:

```
src/Selima/
  AdminBundle/         → Back-office UI at /office
  ApiBundle/           → REST API at /api
  FrontBundle/         → Public front-end at /
  MetierManagerBundle/ → Shared domain: entities, security, repositories, listeners
```

The root PSR-4 namespace is `LCG\` mapped to `src/`. Routes are composed in `config/routes.yaml`
with the prefixes `/api/*`, `/office/*` and `/`.

### Entities

All entities extend an abstract `EntityCore` which adds `dateAdd` / `dateUpd` lifecycle callbacks:

```
EntityCore (abstract)
├── UserCoreEntity (abstract) — email, password, firstName, lastName, enabled, roles
│   ├── Employee     — admin users (ROLE_ADMIN, ROLE_MODERATEUR, ROLE_SUPER_ADMIN)
│   ├── Customer     — customer accounts
│   └── Professional — pro accounts
└── AddressCore      — shared address fields
    ├── AddressCustomer
    └── AddressProfessional
```

### Table prefix

All tables are prefixed `slm_` via a `TablePrefixListener` hooked into Doctrine's
`loadClassMetadata` event. The prefix comes from the `DB_PREFIX` parameter, so entity
`#[ORM\Table(name: ...)]` declarations stay **unprefixed** and the listener adds the prefix at
runtime.

### Security

Three firewalls are configured:

- **`selima_admin`** — session-based form login for `Employee` entities, guards `/office`
  (custom `AdminLoginFormAuthenticator`).
- **`selima_api`** — stateless token authentication via a custom `TokenAuthenticator`,
  guards `/api`.
- **`dev`** — bypasses security for the Symfony profiler.

Role hierarchy: `ROLE_SUPER_ADMIN > ROLE_ADMIN (+ ROLE_MODERATEUR) > ROLE_USER`.

### API documentation

The REST API is documented with **NelmioApiDoc**. JMS Serializer `@Serializer\Groups`
annotations control which fields are exposed per endpoint (e.g. `USER_SINGLE`).

- Stoplight UI: `/api/doc.html`
- OpenAPI JSON: `/api/doc.json`

### Running the backend

```bash
# 1. Environment
cp .env.example .env.local

# 2. Stack
docker compose up -d

# 3. Dependencies
docker exec -t symfony_app_selima_wapp composer install

# 4. Database
bash scripts/sf-cmd.sh doctrine:migrations:migrate
bash scripts/sf-cmd.sh doctrine:fixtures:load   # optional
```

| Service       | URL                                |
| ------------- | ---------------------------------- |
| App (HTTP)    | http://localhost:8080              |
| App (HTTPS)   | https://localhost:8443             |
| phpMyAdmin    | http://localhost:8082              |
| MySQL         | localhost:3306                     |
| API docs (UI) | http://localhost:8080/api/doc.html |
| OpenAPI JSON  | http://localhost:8080/api/doc.json |

A helper script wraps `docker exec` for any Symfony console command:

```bash
bash scripts/sf-cmd.sh cache:clear
bash scripts/sf-cmd.sh doctrine:migrations:diff
bash scripts/sf-cmd.sh doctrine:migrations:migrate
```

### Tests

```bash
# Full suite
docker exec -t symfony_app_selima_wapp php bin/phpunit

# Single file
docker exec -t symfony_app_selima_wapp php bin/phpunit tests/path/to/FooTest.php
```

## Mobile — Expo / React Native

The mobile client is bootstrapped with `create-expo-app` and uses **file-based routing**
(`expo-router`) — every file under `app/` becomes a route.

```bash
# 1. Install dependencies
npm install

# 2. Start the app
npx expo start
```

From the Expo CLI output you can open the app in:

- a [development build](https://docs.expo.dev/develop/development-builds/introduction/)
- an [Android emulator](https://docs.expo.dev/workflow/android-studio-emulator/)
- an [iOS simulator](https://docs.expo.dev/workflow/ios-simulator/)
- [Expo Go](https://expo.dev/go), the sandbox client

Development happens inside the **`app/`** directory. To reset back to a blank starter and
move the example code into `app-example/`:

```bash
npm run reset-project
```

## Environment

Copy `.env.example` to `.env.local` on the backend and configure:

- `DATABASE_URL` — MySQL DSN
- `DB_PREFIX` — table prefix (default `slm_`)
- `MAILER_*` — SMTP settings

The Docker entrypoint copies `.env.example` to `.env.prod.local` inside the container
automatically.

## License

Proprietary — © LCG.
