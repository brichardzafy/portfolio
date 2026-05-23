---
layout: page
title: FiestaClic
description: Marketplace platform for cake designers, bakeries and event caterers — maintainer & fullstack developer
img: assets/img/fiestaclic.png
importance: 4
category: work
giscus_comments: false
---

[FiestaClic](https://www.fiestaclic.com/) is a French marketplace that connects
customers with **cake designers, artisanal pastry chefs, biscuit makers,
bakeries and event caterers**. The platform lets people find a local
professional, browse their creations, get a price estimate, place an order or
book a service for any kind of celebration — weddings, birthdays, baby
showers, corporate events… — and arrange pickup or delivery. On the other side,
it gives **1,500+ professionals** a modern back-office to manage their
catalogue, availability, conversations and orders.

I work on FiestaClic as a **maintainer and fullstack developer**: I keep the
existing platform healthy and continue building new features end-to-end, from
the public marketplace to the pro back-office and the price simulator.

## Tech stack

| Layer              | Technology                                              |
| ------------------ | ------------------------------------------------------- |
| Frontend           | React + Vite, Zustand (state)                           |
| Backend API        | Node.js (Express) — REST API on port `:8080`            |
| Serverless         | Firebase Functions (TypeScript) + **Genkit** for AI flows |
| Auth & data        | Firebase Authentication, Firestore, Firebase Storage    |
| Complementary DB   | Supabase                                                |
| Payments           | Stripe (test & production)                              |
| Affiliation        | Stripe Connect — pro & influencer affiliate program     |
| CRM                | Pipedrive (sales pipeline & lead management)            |
| Maps & places      | Google Maps + Places API                                |
| Transactional mail | Brevo (Sendinblue)                                      |
| Media / images     | Cloudinary                                              |
| Shipping           | Sendcloud (multi-carrier) + Mondial Relay               |
| Runtime            | Node.js 20+                                             |

The codebase is organized as a multi-package project:

- `src/` — main React application (components, pages, Zustand stores,
  services, utils).
- `server/` — Express.js API (modules + shared middlewares, entry point
  `index.js`), including a batch script to generate all invoices.
- `functions/` — Firebase Cloud Functions (TypeScript).
- `src/functions/` — Genkit-based functions for AI-assisted flows.
- `public/` — static assets.

## Third-party integrations

A significant part of the work consists in plugging the platform into a wide
ecosystem of third-party services and keeping those integrations healthy:

- **Shipping — Sendcloud + Mondial Relay** — Sendcloud is used as the
  multi-carrier shipping layer, with Mondial Relay (parcel-shop network) wired
  in for relay-point delivery: address validation, relay-point selection,
  label generation and tracking.
- **Payments — Stripe** — checkout for customer orders, payment methods
  management, refunds, reminders and invoice flows.
- **Affiliation — Stripe Connect** — affiliate program for **professionals
  and influencers**: onboarding of connected accounts, commission tracking on
  qualified orders and automated payouts.
- **CRM — Pipedrive** — synchronization of leads, pros and deals into
  Pipedrive so the sales team can manage the pipeline from a single tool.
- **Maps & geo** — Google Maps + Places for address autocomplete, geocoding
  and proximity search across hundreds of city × category combinations.
- **Email — Brevo** — transactional emails (orders, account, password, pro
  notifications) and marketing campaigns.
- **Media — Cloudinary** — image upload, transformation and CDN delivery for
  pro catalogues and creations.
- **AI — Genkit (Firebase)** — AI-assisted flows wired into Firebase Functions.

## Product surface

FiestaClic is made of several connected surfaces I contribute to:

- **Public marketplace** ([fiestaclic.com](https://www.fiestaclic.com/)) —
  homepage, professional listings by category and city, event-themed landing
  pages (mariage, anniversaire, baby shower, gender reveal, baptême,
  entreprise…), shop pages with online ordering and messaging.
- **Price simulator** ([simulateur.fiestaclic.com](https://simulateur.fiestaclic.com/))
  — interactive estimator that quotes a personalized cake from number of
  servings, type, flavors, decoration and date.
- **FiestaClic Pro** — professionals' back-office to publish their offer,
  manage their agenda, receive and process orders, and exchange with clients.
- **Blog & guides** ([blog.fiestaclic.com](https://www.blog.fiestaclic.com/))
  — SEO content around cake design, recipes and local pastry guides.
- **Help center** ([aide.fiestaclic.com](https://aide.fiestaclic.com/)) —
  documentation for both customers and pros.

## My role

As a **maintainer & fullstack developer**, I cover the full stack on a
production marketplace serving real customers and merchants:

- **Maintenance** — bug triage, regression fixes, security and dependency
  updates, performance monitoring and tuning across the marketplace, the
  simulator and the pro back-office.
- **Frontend** — new pages and features on the public site and the pro
  dashboard, with a focus on responsive UI, accessibility and SEO of the
  category / city / event landing pages.
- **Backend** — APIs, business logic and data modeling for listings, orders,
  messaging, agendas and payments.
- **Search & discovery** — filters by city, category and specialty
  (cake-designer, traiteur, biscuiterie, mariage, anniversaire…), and the
  underlying indexing that powers them.
- **Pro onboarding** — sign-up flow tailored for artisans who aren't
  necessarily tech-savvy, including their shop, agenda and order pipeline.
- **DevOps & releases** — deployments, environment management and
  infrastructure for the marketplace, the simulator subdomain and the help /
  blog subdomains.

## Why it's interesting

FiestaClic is a real **two-sided marketplace** with concrete operational
constraints: artisans need a simple tool to run their shop, customers need a
smooth journey from discovery to order, and the platform has to scale SEO
across hundreds of category × city combinations. That mix makes it a great
playground for fullstack work — UI, APIs, data, SEO and ops all matter.

For a live look at the product, see **[fiestaclic.com](https://www.fiestaclic.com/)**
or try the **[price simulator](https://simulateur.fiestaclic.com/)** directly.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/fiestaclic/home.png" title="FiestaClic — homepage" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/fiestaclic/home.png" title="FiestaClic — price simulator" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    FiestaClic — public marketplace homepage and online price simulator.
</div>
