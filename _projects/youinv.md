---
layout: page
title: YouInv
description: Peppol e-invoicing platform — maintainer & developer
img: assets/img/youinv.png
importance: 1
category: work
related_publications: false
---

[YouInv](https://youinv.com) is an electronic invoicing platform built around the
[Peppol](https://peppol.org) network, the European standard for secure, structured
business-to-business and business-to-government e-invoicing. The product helps companies
create, send and receive compliant electronic invoices (UBL / Peppol BIS 3.0) without
having to deal with the underlying network plumbing themselves.

I joined the project as a **maintainer and developer**, working on both the backend
services that handle Peppol message exchange and the web application used by end-users
to manage their invoices, customers and workflows. My day-to-day work covers:

- **Peppol integration** — sending and receiving invoices through an Access Point,
  XML/UBL document generation, validation against the BIS 3.0 schematrons, and
  troubleshooting interoperability issues with other providers.
- **Backend development** — REST APIs, business logic, data modeling, queueing and
  background jobs for asynchronous invoice processing.
- **Frontend development** — invoice editor, dashboard, customer & document management
  screens, and authentication flows.
- **Maintenance & ops** — bug triage, regression fixes, performance tuning, monitoring,
  deployments and CI/CD pipelines.
- **Compliance** — keeping the platform aligned with evolving Peppol specifications
  and country-specific e-invoicing mandates.

For a live look at the product (UI, features and pricing), see
**[youinv.com](https://youinv.com)** — happy to share more screenshots, architecture
details or specific war stories on request.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/youinv/dashboard.png" title="YouInv dashboard" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/youinv/invoice.png" title="YouInv invoice editor" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    YouInv — dashboard and invoice editor.
</div>

