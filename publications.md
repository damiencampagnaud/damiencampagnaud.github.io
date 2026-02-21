---
layout: default
title: "Publications"
permalink: /publications/
---

<section class="publications-wrapper">

  <section class="search-section">
    <input type="text" id="searchBox" placeholder="Rechercher une publication...">
    <div id="searchResults"></div>
  </section>

  <style>
    /* Tout ton CSS ici (copier le CSS que tu avais dans publications.md) */
  </style>

  <script src="https://unpkg.com/lunr/lunr.js"></script>
  <script>
    /* Tout ton JS ici (copier le JS toggle et search que tu avais dans publications.md) */
  </script>

  <div class="year-block">
    {% assign sorted_publications = site.publications | sort:"date" | reverse %}
    {% for item in sorted_publications %}
      <div class="publication-card" data-url="{{ item.url }}">
        <div class="publication-card-front" onclick="togglePublicationCard(this)">
          <img src="{{ item.image }}" alt="{{ item.title }}">
          <div class="card-overlay">
            <span>{{ item.title }}</span>
            <span>{{ item.date }}</span>
          </div>
        </div>
        <div class="publication-detail">
          {{ item.content }}
          <div class="button-wrapper">
            {% for link in item.links %}
              <a href="{{ link.url }}" target="_blank" class="card-btn {% if forloop.index > 1 %}secondary{% endif %}">
                📄 {{ link.name }}
              </a>
            {% endfor %}
          </div>
        </div>
      </div>
    {% endfor %}
  </div>

</section>
