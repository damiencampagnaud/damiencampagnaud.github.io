---
layout: home
title: "Damien CAMPAGNAUD - Créations Genially et ressources en SVT"
header:
  overlay_image: /assets/images/banniere-minecraft.jpg
  overlay_filter: 0.4
---

## Accès rapide aux Genially

<div class="home-genially-wrapper">
  <div class="home-genially-grid">
    {% for item in site.genially %}
      <a class="home-card" href="{{ item.genially_url }}" target="_blank">
        <img src="{{ item.image }}" alt="{{ item.title }}">
        <div class="home-card-overlay">{{ item.title }}</div>
      </a>
    {% endfor %}
  </div>
</div>
