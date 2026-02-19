---
layout: splash
title: "Damien CAMPAGNAUD - Créations Genially et ressources en SVT"

header:
  overlay_image: /assets/images/banniere-minecraft.jpg
  overlay_filter: 0.4
  overlay_color: "#000"
---
# Sciences de la Vie et de la Terre

Enseignant en collège, je partage ici :

- 🌿 Mes ressources pédagogiques
- 🧬 Mes projets scientifiques
- 🎮 Mes Genially interactifs
- 📄 Mes publications

---

## Accès rapide aux Genially

<div class="genially-grid">
{% for item in site.genially %}
  <div class="genially-card">
    <a href="{{ item.genially_url }}" target="_blank">
      <img src="{{ item.image }}" alt="{{ item.title }}">
    </a>
  </div>
{% endfor %}
</div>
