---
layout: default
title: "Damien CAMPAGNAUD - Créations Genially et ressources en SVT"
---

<style>
/* ================= MOBILE ALIGN FIX ================= */
@media (max-width:768px) {

  .home-genially-grid,
  .home-publication-grid { /* ajout pour la section publication */
    display:flex !important;
    flex-direction:column !important;
    align-items:center !important;
  }

  .home-card,
  .home-publication-card { /* ajout pour la publication */
    width:95% !important;
    margin:0 auto !important;
  }

}
</style>

<main style="max-width:1400px; margin:0 auto; padding:40px;">

  <!-- ================= PUBLICATION A LA UNE ================= -->
  <section class="genially-section">
    <div class="genially-section-inner">
      <h2>Publication à la une</h2>
    </div>
  </section>

  <div class="home-publication-grid">
    {% assign latest_publication = site.publications | sort: "date" | reverse | first %}
    {% if latest_publication %}
      <a class="home-publication-card" href="{{ latest_publication.url }}">
        <img src="{{ latest_publication.image }}" alt="{{ latest_publication.title }}">
        <div class="home-card-overlay">
          <h3>{{ latest_publication.title }}</h3>
          <span>{{ latest_publication.date | date: "%d/%m/%Y" }}</span>
        </div>
      </a>
    {% endif %}
  </div>

  <!-- ================= ACCES RAPIDE AUX GENIALLY ================= -->
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
