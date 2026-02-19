---
layout: home
title: "Damien CAMPAGNAUD - Créations Genially et ressources en SVT"
---

## Mes Genially interactifs

<div class="portfolio-grid">
  {% for project in site.data.projects %}
  <a href="{{ project.url }}" class="portfolio-item" target="_blank">
    <img src="{{ project.image }}" alt="{{ project.title }}">
    <div class="portfolio-overlay">
      <h3>{{ project.title }}</h3>
    </div>
  </a>
  {% endfor %}
</div>

<link rel="stylesheet" href="{{ '/assets/css/home.css' | relative_url }}">
