---
layout: default
title: "Damien CAMPAGNAUD - Créations Genially et ressources en SVT"
---

<header class="page__hero" style="background-image: url('/assets/images/banniere-minecraft.jpg'); height:400px; background-size:cover; background-position:center; position:relative;">
  <div style="position:absolute; right:40px; top:20px; width:160px; height:160px; background-image:url('/assets/images/logoblanc.png'); background-size:contain; background-repeat:no-repeat;"></div>
</header>

<main style="max-width:1400px; margin:0 auto; padding:40px;">

  <h2>Accès rapide aux Genially</h2>

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
