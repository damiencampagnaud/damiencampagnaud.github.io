---
layout: default
title: "Damien CAMPAGNAUD - Créations Genially et ressources en SVT"
---

<style>
/* ================= MOBILE ALIGN FIX ================= */
@media (max-width:768px) {

  .home-genially-grid,
  .home-publication-grid {
    display:flex !important;
    flex-direction:column !important;
    align-items:center !important;
  }

  .home-card,
  .home-publication-card {
    width:95% !important;
    margin:0 auto !important;
  }

  /* === FIX MOBILE pour publication à la une === */
  .home-publication-card .publication-card-front,
  .home-publication-card .publication-detail {
    width:100% !important;
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
      <div class="home-publication-card publication-card collapsed" data-url="{{ latest_publication.url }}">

        <div class="publication-card-front" onclick="toggleCard(this)">
          <img src="{{ latest_publication.image }}" alt="{{ latest_publication.title }}">
          <div class="card-overlay">
            <h3>{{ latest_publication.title }}</h3>
            <span>{{ latest_publication.date | date: "%d/%m/%Y" }}</span>
          </div>
        </div>

        <div class="publication-detail">
          {{ latest_publication.content }}

          <div class="button-wrapper">
            {% if latest_publication.links %}
              {% for link in latest_publication.links %}
                <a href="{{ link.url }}" target="_blank" class="card-btn {% if link.secondary %}secondary{% endif %}">
                  {{ link.name }}
                </a>
              {% endfor %}
            {% endif %}
          </div>
        </div>

      </div>
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

<script>
/* ================= TOGGLE COLLAPSED PUBLICATION ================= */
function toggleCard(element) {
  const card = element.closest('.publication-card');
  card.classList.toggle('collapsed');
}
</script>
