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
}

.genially-card {
  width:calc(33.333% - 20px);
  transition:0.3s;
}

.genially-card.expanded {
  width:100%;
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
}

.genially-card.expanded .genially-detail {
  display:block;
}

/* ================= BUTTONS ================= */

.card-btn {
  display:inline-block;
  margin:10px 10px 0 0;
  padding:10px 18px;
  border-radius:8px;
  background:#159957;
  color:white;
  text-decoration:none;
  font-weight:bold;
}

.card-btn.secondary {
  background:#3b82f6;
}

.card-btn:hover {
  opacity:0.9;
}

</style>

<script src="https://unpkg.com/lunr/lunr.js"></script>

<script>
let storeData = []

fetch('/search.json')
  .then(response => response.json())
  .then(data => {

    storeData = data

    const idx = lunr(function () {
      this.ref('url')
      this.field('title')
      this.field('niveau')
      this.field('content')
      data.forEach(doc => this.add(doc))
    })

    document.getElementById('searchBox').addEventListener('input', function() {

      const query = this.value.trim()

      if(query === ""){
        document.getElementById('searchResults').innerHTML = ""
        return
      }

      const results = idx.search(query)

      let output = ""

      results.forEach(result => {

        const match = storeData.find(d => d.url === result.ref)

        output += `
          <div class="genially-card expanded">
            <div class="genially-card-front">
              <img src="${match.image}" alt="${match.title}">
              <div class="card-overlay">
                <h3>${match.title}</h3>
                <p>${match.niveau}</p>
              </div>
            </div>

            <div class="genially-detail" style="display:block;">
              ${match.content}
              <br><br>
              <a href="${match.genially_url}" target="_blank" class="card-btn">
                🎮 Ouvrir le Genially
              </a>
              <a href="${match.pdf_url}" target="_blank" class="card-btn secondary">
                📄 Fiche activité PDF
              </a>
            </div>
          </div>
        `
      })

      document.getElementById('searchResults').innerHTML = output

    })

  })

/* ================= NIVEAU TOGGLE ================= */

function toggleNiveau(element) {
  const content = element.nextElementSibling;
  content.classList.toggle("open");
}

/* ================= CARD TOGGLE ================= */

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

          <div class="genially-card">

            <div class="genially-card-front" onclick="toggleCard(this)">
              <img src="{{ item.image }}" alt="{{ item.title }}">
              <div class="card-overlay">
                <h3>{{ item.title }}</h3>
                <p>{{ item.niveau }}</p>
              </div>
            </div>

            <div class="genially-detail">
              {{ item.content }}
              <br><br>
              <a href="{{ item.genially_url }}" target="_blank" class="card-btn">
                🎮 Ouvrir le Genially
              </a>
              <a href="{{ item.pdf_url }}" target="_blank" class="card-btn secondary">
                📄 Fiche activité PDF
              </a>
            </div>

          </div>

        {% endif %}
      {% endfor %}

      </div>

    </div>

  </div>

{% endfor %}

</section>
