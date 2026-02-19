---
layout: single
title: "Ressources"
permalink: /ressources/
---

## Index des niveaux

- [6ème](#6eme)
- [5ème](#5eme)
- [4ème](#4eme)
- [3ème](#3eme)

---

<input type="text" id="searchInput" placeholder="Rechercher un Genially..." style="width:100%;padding:10px;margin:20px 0;">

---

## 6ème
<div id="6eme" class="niveau-section">
{% for item in site.genially %}
{% if item.niveau == "6ème" %}
  <div class="genially-card searchable">
    <h3>{{ item.title }}</h3>
    <p>{{ item.niveau }}</p>
    <a href="{{ item.genially_url }}">🎮 Ouvrir le Genially</a><br>
    <a href="{{ item.url }}">📄 Voir la ressource</a>
  </div>
{% endif %}
{% endfor %}
</div>

---

## 5ème
<div id="5eme" class="niveau-section">
{% for item in site.genially %}
{% if item.niveau == "5ème" %}
  <div class="genially-card searchable">
    <h3>{{ item.title }}</h3>
    <p>{{ item.niveau }}</p>
    <a href="{{ item.genially_url }}">🎮 Ouvrir le Genially</a><br>
    <a href="{{ item.url }}">📄 Voir la ressource</a>
  </div>
{% endif %}
{% endfor %}
</div>

---

## 4ème
<div id="4eme" class="niveau-section">
{% for item in site.genially %}
{% if item.niveau == "4ème" %}
  <div class="genially-card searchable">
    <h3>{{ item.title }}</h3>
    <p>{{ item.niveau }}</p>
    <a href="{{ item.genially_url }}">🎮 Ouvrir le Genially</a><br>
    <a href="{{ item.url }}">📄 Voir la ressource</a>
  </div>
{% endif %}
{% endfor %}
</div>

---

## 3ème
<div id="3eme" class="niveau-section">
{% for item in site.genially %}
{% if item.niveau == "3ème" %}
  <div class="genially-card searchable">
    <h3>{{ item.title }}</h3>
    <p>{{ item.niveau }}</p>
    <a href="{{ item.genially_url }}">🎮 Ouvrir le Genially</a><br>
    <a href="{{ item.url }}">📄 Voir la ressource</a>
  </div>
{% endif %}
{% endfor %}
</div>

<script>
document.getElementById("searchInput").addEventListener("keyup", function() {
  let filter = this.value.toLowerCase();
  let cards = document.querySelectorAll(".searchable");

  cards.forEach(function(card) {
    let text = card.innerText.toLowerCase();
    card.style.display = text.includes(filter) ? "" : "none";
  });
});
</script>
