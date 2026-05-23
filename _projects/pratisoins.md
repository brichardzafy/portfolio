---
layout: page
title: Pratisoins
description: French marketplace for wellness practitioners — maintainer & fullstack developer
img: assets/img/pratisoins.png
importance: 5
category: work
giscus_comments: false
---

[Pratisoins](https://pratisoins.fr) is a French marketplace that connects
patients with **certified wellness practitioners** — reflexology, kinesiology,
yoga, shiatsu, hypnotherapy, naturopathy, traditional Chinese medicine, energy
practices and more. The platform helps users find a trusted local practitioner
near them, browse profiles by specialty, situation (addictions, chronic
fatigue, OCD…) or city, and book an appointment either at the practitioner's
office or remotely. On the other side, practitioners get a dedicated pro
back-office at [pro.pratisoins.fr](https://pro.pratisoins.fr) to manage their
visibility, agenda and patients.

I work on Pratisoins as a **maintainer and fullstack developer** — covering
the public website, the pro back-office and the API. One of my main
contributions has been **optimizing the search by distance using geocoded
data**, so patients can quickly find the closest practitioners matching their
criteria across the Pays de la Loire region (200+ practitioners, 15+
specialties, 50+ cities).

## Tech stack

| Layer            | Technology                                         |
| ---------------- | -------------------------------------------------- |
| Backend          | Node.js 22+ (Express) — REST API on port `:5000`   |
| ORM              | Prisma (with concatenated multi-file schema)       |
| Database         | MySQL (`origami_db`)                               |
| Frontend         | React (public site + pro back-office)              |
| API host         | [api.pratisoins.fr](https://api.pratisoins.fr)     |
| Media / assets   | Served from the API (`/medias/contents/…`)         |
| Hosting & data   | French servers, GDPR-compliant                     |
| Runtime          | Node.js v22.8.0 or higher                          |

## Geo search optimization

The marketplace is built around a **find-the-closest-practitioner** experience,
which means search performance and accuracy directly drive the product. My
work on this part includes:

- Storing **geocoded coordinates** (lat/lng) for every practitioner and city
  so distance can be computed at query time.
- Implementing **distance-based ranking** in the search endpoint to return
  practitioners sorted by proximity to the patient's location, with optional
  filters by specialty, situation and modality (in-office or remote).
- Optimizing the underlying SQL queries (indexing + bounding-box pre-filter
  before the precise distance calculation) so search stays fast as the
  catalogue grows.
- Exposing clean, paginated results to the frontend so the public site can
  render city pages, specialty pages and free search consistently.

## Other contributions

- **Backend maintenance** — Prisma schema and migrations (`prisma:generate`,
  `prisma:migrate`, `prisma:concat`), bug fixes, dependency updates and
  troubleshooting migration drift.
- **API features** — endpoints powering the public site (practitioner
  listings, specialties, situations, cities) and the pro back-office
  (profiles, agendas, content).
- **Pro back-office** ([pro.pratisoins.fr](https://pro.pratisoins.fr)) —
  features that let practitioners manage their visibility, appointments and
  patient interactions.
- **Public marketplace** — homepage, search, practitioner profile pages,
  per-city and per-specialty SEO landing pages.

## Product surface

- **Patient site** ([pratisoins.fr](https://pratisoins.fr)) — discovery,
  search and booking.
- **Pro back-office** ([pro.pratisoins.fr](https://pro.pratisoins.fr)) —
  practitioner dashboard.
- **API** ([api.pratisoins.fr](https://api.pratisoins.fr)) — REST backend
  powering both surfaces.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/pratisoins/home.png" title="Pratisoins — homepage" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/pratisoins/search.png" title="Pratisoins — practitioner search" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Pratisoins — homepage and practitioner search.
</div>
