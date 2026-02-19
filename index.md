---
layout: splash
title: "Damien CAMPAGNAUD - Créations Genially et ressources en SVT"
header:
  overlay_image: /assets/images/banniere-minecraft.jpg
  overlay_filter: 0.4
---

## Mes Genially interactifs

<div class="genially-grid">
{% for item in site.genially %}
  <div class="genially-card">
    <a href="{{ item.genially_url }}" target="_blank">
      <img src="{{ item.image }}" alt="{{ item.title }}">
      <div class="genially-title">{{ item.title }}</div>
    </a>
  </div>
{% endfor %}
</div>
