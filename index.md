---
layout: home-genially
title: "Damien CAMPAGNAUD - Créations Genially et ressources en SVT"
header:
  overlay_image: /assets/images/banniere-minecraft.jpg
  overlay_filter: 0.4
---

<div class="home-genially-grid">
  {% for item in site.genially %}
    <a href="{{ item.genially_url }}" target="_blank" class="home-card">
      <img src="{{ item.image }}" alt="{{ item.title }}">
      <div class="home-card-overlay">
        <h2>{{ item.title }}</h2>
        <p>{{ item.niveau }}</p>
      </div>
    </a>
  {% endfor %}
</div>
