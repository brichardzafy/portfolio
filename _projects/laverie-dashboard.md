---
layout: page
title: Laundries Dashboard
description: Next.js back-office for managing connected laundries and their mobile app
img: assets/img/laverie.png
importance: 2
category: work
giscus_comments: false
---

**Laundries Dashboard** is the back-office used to operate a fleet of connected
laundries and their companion mobile application. On this project I work as a
**Front-End & Mobile developer**: I design and build the screens, the user flows
and the whole client layer (web + mobile) that consumes the business APIs.

The back-office centralizes laundry management, contracts, after-sales support,
intervention scheduling, usage analytics (cycles, devices, users) as well as a
real-time map of all sites.

## Tech stack

| Category       | Technology                          |
| -------------- | ----------------------------------- |
| Framework      | Next.js 15 (App Router)             |
| Language       | TypeScript 5+                       |
| Styling        | Tailwind CSS 3 (mobile-first)       |
| State          | Zustand 5                           |
| Data fetching  | TanStack React Query 5              |
| Auth           | NextAuth 4                          |
| Forms          | React Hook Form 7                   |
| Maps           | Leaflet + React-Leaflet             |
| Rich editor    | TipTap 3                            |
| Icons          | Lucide React                        |
| PDF export     | jsPDF                               |
| Runtime        | Node.js 20+                         |

## Main modules

- **Laundries & map** — list view, per-site detail pages and a Leaflet-based
  map view of the whole fleet.
- **Contracts & orders** — customer contract management and order tracking.
- **Scheduling & after-sales** — intervention planning and built-in support
  messaging.
- **Statistics** — dashboards for cycles, devices and users.
- **Script library, news, technical documentation, specifications** — internal
  tools for the teams.
- **Profiles & settings** — user management, profiles and configuration.

## Front-end architecture

- **Next.js App Router**: file-based routing under `app/`, nested layouts and
  dedicated API routes (`api/ai`, `api/settings`).
- **React contexts** (`contexts/`): cross-component shared data (brands,
  models, contracts…).
- **Zustand stores** (`stores/` + `hooks/zustand/`): global application state
  (selected laundry, forms, current user).
- **TanStack Query**: fetching, caching and synchronization of API data.
- **Components** organized by business domain (`laundry/`, `scheduling/`,
  `profiles/`, `statistics/`…) plus a set of generic UI primitives (`ui/`).

## Code standards

The project enforces strict rules to keep the codebase healthy over time:

- **650 lines** — ideal per-file limit
- **650–670** — soft warning
- **670–700** — strong warning, refactoring recommended
- **700+** — blocking error (CI)

Conventions in place:

- Components in `PascalCase`, one component per file.
- Hooks in `camelCase` prefixed with `use`.
- Tailwind CSS only, **mobile-first** approach.
- Props typed with explicit TypeScript interfaces.
- Any component over 400 lines is split into a sub-folder.

A `npm run check:file-sizes` script enforces these thresholds in CI.

## My role

As a **Front-End & Mobile developer**, I am in charge of:

- Implementing the Next.js (App Router) screens and the user experience of the
  back-office.
- Integrating the map view, complex forms (React Hook Form), the TipTap rich
  editor and PDF exports.
- Managing application state (Zustand) and the data cache (TanStack Query).
- Applying and maintaining the code standards (file size limits, decomposition,
  conventions).
- Building the mobile counterpart of the platform, consistent with the
  back-office.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/laverie/dashboard.png" title="Laundries Dashboard — global overview" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/laverie/devices.png" title="Laundries Dashboard — devices view" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Laundries Dashboard — global overview and per-laundry devices view.
</div>
