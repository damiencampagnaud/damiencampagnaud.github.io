---
layout: default
title: "Ressources"
permalink: /ressources/
---

<section class="search-section">
  <input type="text" id="searchBox" placeholder="Rechercher un Genially...">
  <div id="searchResults"></div>
</section>

<section class="niveau-wrapper">

{% assign niveaux = "6ème,5ème,4ème,3ème" | split: "," %}

{% for niveau in niveaux %}

  <div class="niveau-block">

    <div class="niveau-header" onclick="toggleNiveau(this)">
      <h2>{{ niveau }}</h2>
    </div>

    <div class="niveau-content">

      <div class="genially-grid">

      {% for item in site.genially %}
        {% if item.niveau == niveau %}

          <div class="genially-card">

            <div class="genially-card-front" onclick="toggleCard(this)">
              <img src="{{ item.image }}" alt="{{ item.title }}">
              <div class="card-overlay">
                <h3>{{ item.title }}</h3>
                <p>{{ item.niveau }}</p>
              </div>
            </div>

            <div class="genially-card-back">
              {{ item.content }}
              <a href="{{ item.genially_url }}" target="_blank" class="card-btn">🎮 Ouvrir le Genially</a>
            </div>

          </div>

        {% endif %}
      {% endfor %}

      </div>

    </div>

  </div>

{% endfor %}

</section>

<script src="https://unpkg.com/lunr/lunr.js"></script>
<script src="/search.js"></script>

<script>
function toggleNiveau(element) {
  const content = element.nextElementSibling;
  content.classList.toggle("open");
}

function toggleCard(element) {
  const card = element.parentElement;
  card.classList.toggle("expanded");
}
</script>
