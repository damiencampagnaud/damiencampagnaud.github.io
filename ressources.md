---
layout: default
title: "Ressources"
permalink: /ressources/
---

<h1>Index des niveaux</h1>

<ul>
  <li><a href="#6eme">6ème</a></li>
  <li><a href="#5eme">5ème</a></li>
  <li><a href="#4eme">4ème</a></li>
  <li><a href="#3eme">3ème</a></li>
</ul>

<input type="text" id="searchBox" placeholder="Rechercher un Genially..." style="width:100%;padding:10px;margin:20px 0;">
<div id="searchResults"></div>

<script src="https://unpkg.com/lunr/lunr.js"></script>
<script>
fetch('/search.json')
  .then(response => response.json())
  .then(data => {
    const idx = lunr(function () {
      this.ref('url')
      this.field('title')
      this.field('niveau')
      this.field('content')

      data.forEach(doc => this.add(doc))
    })

    document.getElementById('searchBox').addEventListener('input', function() {
      const results = idx.search(this.value)
      let output = ""

      results.forEach(result => {
        const match = data.find(d => d.url === result.ref)
        output += `
          <div class="genially-card" style="border:1px solid #ddd; padding:15px; margin-bottom:10px; border-radius:10px;">
            <h3>${match.title}</h3>
            <p>${match.niveau}</p>
            <a href="${match.genially_url}" target="_blank">🎮 Ouvrir le Genially</a><br>
            <a href="${match.url}">📄 Voir la ressource</a>
          </div>
        `
      })

      document.getElementById('searchResults').innerHTML = output
    })
  })
</script>

---

## 6ème
<div id="6eme" class="niveau-section">
{% for item in site.genially %}
{% if item.niveau == "6ème" %}
  <div class="genially-card">
    <h3>{{ item.title }}</h3>
    <p>{{ item.niveau }}</p>
    <a href="{{ item.genially_url }}" target="_blank">🎮 Ouvrir le Genially</a><br>
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
  <div class="genially-card">
    <h3>{{ item.title }}</h3>
    <p>{{ item.niveau }}</p>
    <a href="{{ item.genially_url }}" target="_blank">🎮 Ouvrir le Genially</a><br>
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
  <div class="genially-card">
    <h3>{{ item.title }}</h3>
    <p>{{ item.niveau }}</p>
    <a href="{{ item.genially_url }}" target="_blank">🎮 Ouvrir le Genially</a><br>
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
  <div class="genially-card">
    <h3>{{ item.title }}</h3>
    <p>{{ item.niveau }}</p>
    <a href="{{ item.genially_url }}" target="_blank">🎮 Ouvrir le Genially</a><br>
    <a href="{{ item.url }}">📄 Voir la ressource</a>
  </div>
{% endif %}
{% endfor %}
</div>
