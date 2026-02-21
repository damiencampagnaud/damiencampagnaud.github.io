---
layout: default
title: "Ressources"
permalink: /ressources/
---

<section class="search-section">
  <input type="text" id="searchBox" placeholder="Rechercher un Genially...">
  <div id="searchResults"></div>
</section>

<style>

/* ================= SEARCH ================= */

.search-section {
  text-align:center;
  margin-bottom:40px;
}

#searchBox {
  width:60%;
  max-width:500px;
  padding:12px;
  border-radius:8px;
  border:1px solid #ccc;
  font-size:16px;
}

#searchResults {
  margin-top:30px;
}

/* ================= NIVEAUX ================= */

.niveau-header {
  background:white;
  padding:18px;
  border-radius:12px;
  cursor:pointer;
  margin-bottom:15px;
  box-shadow:0 4px 10px rgba(0,0,0,0.08);
}

.niveau-content {
  display:none;
  margin-bottom:40px;
}

.niveau-content.open {
  display:block;
}

/* ================= GRID ================= */

.genially-grid {
  display:flex;
  flex-wrap:wrap;
  gap:20px;
  justify-content:center;
}

.genially-card {
  flex: 1 1 300px;
  max-width:350px;
  transition:0.3s;
}

.genially-card.expanded {
  flex: 1 1 100%;
  max-width:100%;
}

/* ================= FRONT ================= */

.genially-card-front {
  position:relative;
  cursor:pointer;
  border-radius:12px;
  overflow:hidden;
  box-shadow:0 4px 12px rgba(0,0,0,0.1);
}

.genially-card-front img {
  width:100%;
  display:block;
}

.card-overlay {
  position:absolute;
  bottom:0;
  width:100%;
  background:rgba(0,0,0,0.6);
  color:white;
  padding:10px;
}

/* ================= DETAIL ================= */

.genially-detail {
  display:none;
  background:white;
  padding:30px;
  margin-top:15px;
  border-radius:12px;
  box-shadow:0 6px 18px rgba(0,0,0,0.1);
  text-align:center;
}

.genially-card.expanded .genially-detail {
  display:block;
}

/* ================= BUTTONS (FIX CAYMAN PROPRE) ================= */

.button-wrapper {
  margin-top:20px;
  display:flex;
  flex-wrap:wrap;
  justify-content:center;
  gap:15px;
}

/* On écrase totalement le style du thème Cayman */
.genially-detail .button-wrapper a.card-btn {
  all: unset;
  display:inline-flex;
  align-items:center;
  justify-content:center;

  height:44px;
  padding:0 22px;
  border-radius:8px;

  background:#159957;
  color:white;
  font-weight:bold;
  font-size:15px;
  text-decoration:none;
  cursor:pointer;

  box-sizing:border-box;
}

.genially-detail .button-wrapper a.card-btn.secondary {
  background:#3b82f6;
}

.genially-detail .button-wrapper a.card-btn:hover {
  opacity:0.9;
}

/* ================= MOBILE ================= */

@media (max-width:768px) {

  #searchBox {
    width:90%;
  }

  .genially-card {
    flex:1 1 100%;
    max-width:100%;
  }

}

</style>

<script src="https://unpkg.com/lunr/lunr.js"></script>

<script>

fetch('/search-genially.json')
  .then(response => response.json())
  .then(data => {

    const idx = lunr(function () {
      this.ref('url')
      this.field('title')
      this.field('content')
      data.forEach(doc => this.add(doc))
    })

    document.getElementById('searchBox').addEventListener('input', function() {

      const query = this.value.trim()
      const resultsContainer = document.getElementById('searchResults')
      resultsContainer.innerHTML = ""

      if(query === "") return

      const results = idx.search(query)

      results.forEach(result => {

        const originalCard = document.querySelector(
          '.genially-card[data-url="' + result.ref + '"]'
        )

        if(originalCard){

          const clone = originalCard.cloneNode(true)
          clone.classList.add("expanded")
          clone.style.maxWidth = "100%"

          resultsContainer.appendChild(clone)

        }

      })

    })

  })

function toggleNiveau(element) {
  const content = element.nextElementSibling;
  content.classList.toggle("open");
}

function toggleCard(element) {

  const card = element.closest('.genially-card');

  document.querySelectorAll('.genially-card').forEach(c => {
    if (c !== card) {
      c.classList.remove('expanded');
    }
  });

  card.classList.toggle("expanded");

}

</script>

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

          <div class="genially-card" data-url="{{ item.url }}">

            <div class="genially-card-front" onclick="toggleCard(this)">
              <img src="{{ item.image }}" alt="{{ item.title }}">
              <div class="card-overlay">
                <h3>{{ item.title }}</h3>
                <p>{{ item.niveau }}</p>
              </div>
            </div>

            <div class="genially-detail">

              {{ item.content }}

              <div class="button-wrapper">

                <a href="{{ item.genially_url }}" target="_blank" class="card-btn">
                  🎮 Ouvrir le Genially
                </a>

                {% if item.pdf_files %}
                  {% for pdf in item.pdf_files %}
                    <a href="{{ pdf.url }}" target="_blank" class="card-btn secondary">
                      📄 {{ pdf.name }}
                    </a>
                  {% endfor %}
                {% endif %}

              </div>

            </div>

          </div>

        {% endif %}
      {% endfor %}

      </div>

    </div>

  </div>

{% endfor %}

</section>
