---
layout: page
title: Michalak
description: PrestaShop 1.6 store for pastry chef Christophe Michalak — backend developer & maintainer (custom modules for shipping planning, order slips and POS sync)
img: assets/img/michalak.png
importance: 6
category: work
giscus_comments: false
---

[christophemichalak.com](https://www.christophemichalak.com/) is the official online store of
French pastry chef **Christophe Michalak** — cakes, pâtisserie, viennoiserie, chocolate, épicerie
and masterclasses, with home delivery via Chronopost. The site runs on **PrestaShop 1.6** with a
custom theme (`michalak-v2`) and a stack of business-specific modules.

I work on Michalak as the **backend developer and maintainer**, building the custom PHP modules
and overrides that the shop needs on top of stock PrestaShop — shipping logic by product
category, accounting documents tied to specific order states, and integrations with the client's
ERP.

## Tech stack

| Layer            | Technology                                                  |
| ---------------- | ----------------------------------------------------------- |
| Platform         | **PrestaShop 1.6**                                          |
| Language         | PHP (PrestaShop overrides + custom modules)                 |
| Theme            | `themes/michalak-v2/` — Smarty + Sass / Compass + JS         |
| Database         | MySQL (via Docker / phpMyAdmin)                             |
| Infra (local)    | Docker Compose — store on `:8080`, phpMyAdmin on `:8081`    |
| Deployment       | GitLab CI → **git-ftp** auto-deploy on push                 |
| POS integration  | **myPOS** (REST API call + inbound webhook)                 |

## My work — custom modules & overrides

The repo only tracks the customized parts of PrestaShop; the PS core lives in a Docker volume.
My contributions revolve around three business needs:

### 1. Shipping planning by product category

Custom modules that drive **delivery scheduling rules per product category** — some items
(fresh pastries, cakes, viennoiserie) only ship on specific days, while épicerie / chocolate
items follow the standard Chronopost flow. The carrier selection, available delivery dates
and cut-off times are computed from the categories present in the cart.

- `modules/lvx_categoryplandelivery/` — category → shipping plan mapping
- `modules/lvx_shippingplanningdelivery/` — front-office delivery date picker driven by cart content
- `modules/planningdeliverybycarrier/` — carrier-level planning rules

### 2. Order slip generator tied to order status

A PDF / document generation flow that produces **order slips for specific PrestaShop order
states**, so the back-office team can issue the right accounting document automatically when
an order transitions to a given status (shipped, delivered, refunded…).

- `modules/lvx_orderslipgenerator/` — order slip generation hooked on order-state changes
- `modules/lvx_refordergenerator/` — re-order document flow
- `modules/statscompta/` — accounting / VAT stats module

### 3. myPOS integration

A connector module (`modules/lvx_mypos_link/`) that bridges PrestaShop with the client's
**myPOS** point-of-sale system:

- **Outbound** — when an order reaches a configured PrestaShop order state, the module
  forwards it to the configured **webhook URL** with a secured token, so myPOS receives the
  order automatically.
- **Carrier mapping** — a back-office configuration screen maps each PrestaShop carrier to
  its myPOS counterpart, so the right shipping method is sent through.
- **Configurable triggers** — the order state, the eligible carrier and the webhook
  endpoint / token are all configurable from the admin panel (no code change needed when
  the workflow evolves).
- **Inbound** — exposes a front controller so myPOS can call back into PrestaShop to update
  order references and statuses.

## Other contributions

- **Module maintenance** across the custom module ecosystem (`lvx_*`, `recrutement`,
  `coursereservation`, `lvx_masterclassbloc`, `lvx_coffretmanager`…).
- **PrestaShop overrides** in `override/` and custom front controllers under
  `controllers/front/` for business-specific routes.
- **One-shot SQL migrations** in `migrations/` — applied manually via the browser when a
  release needs schema changes outside the PrestaShop installer.
- Theme touch-ups in `themes/michalak-v2/` (Smarty templates + Sass compiled with Compass).

## Local environment

```bash
docker-compose up -d
```

| Service     | URL                                  |
| ----------- | ------------------------------------ |
| Store       | http://localhost:8080                |
| phpMyAdmin  | http://localhost:8081                |
| Admin panel | http://localhost:8080/admin4577      |

DB credentials: user `root`, password `admin`.

### Theme CSS workflow

The theme uses Sass / Compass. From `themes/michalak-v2/`:

```bash
compass watch    # dev — recompiles on save
compass compile  # one-time build
```

Entry point: `sass/michalack_v2/style.scss` → `css/michalack_v2.min.css`.

## Deployment

| Branch    | Environment                                              |
| --------- | -------------------------------------------------------- |
| `PREPROD` | [michalak-2.projet-client.com](https://michalak-2.projet-client.com/) |
| `PROD`    | [christophemichalak.com](https://www.christophemichalak.com/)         |

GitLab CI deploys automatically via **git-ftp** on push to either branch.

## Repository structure

```
themes/michalak-v2/   # active theme (Smarty, Sass, JS)
override/             # PHP class & controller overrides
controllers/front/    # custom front-office controllers
modules/lvx_*/        # custom business modules (delivery, order slips, myPOS…)
modules/recrutement/  # custom careers / job application module
modules/statscompta/  # accounting stats module
migrations/           # one-shot SQL scripts (run manually via browser)
```
