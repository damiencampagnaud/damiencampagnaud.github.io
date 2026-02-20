---
layout: default
title: "Damien CAMPAGNAUD - Créations Genially et ressources en SVT"
---

<!-- Favicon -->
<link rel="icon" href="{{ '/assets/images/Logo.png' | relative_url }}" type="image/png">

<style>
/* ================= MOBILE ALIGN FIX ================= */
@media (max-width:768px) {

  .home-genially-grid {
    display:flex !important;
    flex-direction:column !important;
    align-items:center !important;
  }

  .home-card {
    width:95% !important;
    margin:0 auto !important;
  }

}
</style>

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
