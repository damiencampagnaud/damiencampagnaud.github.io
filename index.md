---
layout: default
title: "Damien CAMPAGNAUD - Créations Genially et ressources en SVT"
---
<main style="max-width:1400px; margin:0 auto; padding:40px;">

  <section class="genially-section">
  <div class="genially-section-inner">
    <h2>Accès rapide aux Genially</h2>
  </div>
</section>

  <div class="home-genially-grid">
    {% for item in site.genially %}
      <a class="home-card" href="{{ item.genially_url }}" target="_blank">
        <img src="{{ item.image }}" alt="{{ item.title }}">
        <div class="home-card-overlay">
          <h3>{{ item.title }}</h3>
          <p>{{ item.niveau }}</p>
        </div>
      </a>
    {% endfor %}
  </div>

</main>
