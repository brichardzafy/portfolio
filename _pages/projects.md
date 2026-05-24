---
layout: page
title: Projects
permalink: /projects/
description: A selection of things I've built — from production fullstack apps to weekend experiments.
nav: true
nav_order: 3
display_categories: [work, fun, own]
horizontal: false
---

<p>
  Here is a curated collection of projects I've worked on as a fullstack developer.
  The <strong>work</strong> section gathers professional and client projects where I focused on
  building reliable, well-architected web applications — covering frontend, backend, APIs, databases
  and deployment. The <strong>fun</strong> section showcases personal side projects and experiments
  I built to explore new technologies, sharpen specific skills, or simply scratch an itch.
</p>

<p>
  Each card below links to a dedicated page with screenshots, the tech stack used, the problems
  I had to solve, and the lessons I took away from the build. Feel free to dive in — and if you'd
  like to chat about any of them, you can reach me via the links on the <a href="{{ '/' | relative_url }}">about page</a>.
</p>

<!-- pages/projects.md -->
<div class="projects">
{% if site.enable_project_categories and page.display_categories %}
  <!-- Display categorized projects -->
  {% for category in page.display_categories %}
  <a id="{{ category }}" href=".#{{ category }}">
    <h2 class="category">{{ category }}</h2>
  </a>
  {% assign categorized_projects = site.projects | where: "category", category %}
  {% assign sorted_projects = categorized_projects | sort: "importance" %}
  <!-- Generate cards for each project -->
  {% if page.horizontal %}
  <div class="container">
    <div class="row row-cols-1 row-cols-md-2">
    {% for project in sorted_projects %}
      {% include projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
  {% endfor %}

{% else %}

<!-- Display projects without categories -->

{% assign sorted_projects = site.projects | sort: "importance" %}

  <!-- Generate cards for each project -->

{% if page.horizontal %}

  <div class="container">
    <div class="row row-cols-1 row-cols-md-2">
    {% for project in sorted_projects %}
      {% include projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
{% endif %}
</div>
