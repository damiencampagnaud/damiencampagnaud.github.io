---
layout: default
title: "Publications"
permalink: /publications/
---

<section class="search-section">
  <input type="text" id="searchBox" placeholder="Rechercher une publication...">
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

/* ================= YEAR ================= */
.year-header {
  text-align:center;
  font-weight:bold;
  font-size:24px;
  color:black;
  margin-bottom:20px;
}

/* ================= GRID ================= */
.publication-grid {
  display:flex;
  flex-wrap:wrap;
  gap:20px;
  justify-content:center;
}

.publication-card {
  flex: 1 1 300px;
  max-width:350px;
  transition:0.3s;
}

.publication-card.expanded {
  flex:1 1 100%;
  max-width:100%;
}

/* ================= FRONT ================= */
.publication-card-front {
  position:relative;
  cursor:pointer;
  border-radius:12px;
  overflow:hidden;
  box-shadow:0 4px 12px rgba(0,0,0,0.1);
}

.publication-card-front img {
  width:100%;
  display:block;
}

/* Overlay au hover */
.card-overlay {
  position:absolute;
  top:0;
  left:0;
  width:100%;
  height:100%;
  display:flex;
  flex-direction:column;
  justify-content:center;
  align-items:center;
  background:rgba(0,0,0,0.6);
  color:white;
  opacity:0;
  transition:0.3s;
  text-align:center;
  padding:10px;
}

.publication-card-front:hover .card-overlay {
  opacity:1;
}

.card-overlay h3 {
  font-weight:bold;
  font-size:18px;
  margin-bottom:5px;
}

.card-overlay p {
  font-weight:normal;
  font-size:14px;
  margin:0;
}

/* ================= DETAIL ================= */
.publication-detail {
  display:none;
  background:white;
  padding:30px;
  margin-top:15px;
  border-radius:12px;
  box-shadow:0 6px 18px rgba(0,0,0,0.1);
  text-align:center;
}

.publication-card.expanded .publication-detail {
  display:block;
}

/* ================= BUTTONS ================= */
.button-wrapper {
  margin-top:20px;
  display:flex;
  flex-wrap:wrap;
  justify-content:center;
  gap:15px;
}

.publication-detail .button-wrapper a.card-btn {
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

.publication-detail .button-wrapper a.card-btn.secondary {
  background:#3b82f6;
}

.publication-detail .button-wrapper a.card-btn:hover {
  opacity:0.9;
}

/* ================= MOBILE ================= */
@media (max-width:768px) {
  #searchBox {
    width:90%;
  }

  .publication-card {
    flex:1 1 100%;
    max-width:100%;
  }
}

</style>

<script src="https://unpkg.com/lunr/lunr.js"></script>

<script>
function toggleCard(element) {
  const card = element.closest('.publication-card');
  document.querySelectorAll('.publication-card').forEach(c => {
    if (c !== card) c.classList.remove('expanded');
  });
  card.classList.toggle('expanded');
}

// Recherche
fetch('/search.json')
  .then(response => response.json())
  .then(data => {
    const idx = lunr(function () {
      this.ref('url')
      this.field('title')
      this.field('content')
      data.forEach(doc => this.add(doc))
    });

    document.getElementById('searchBox').addEventListener('input', function() {
      const query = this.value.trim();
      const resultsContainer = document.getElementById('searchResults');
      resultsContainer.innerHTML = "";
      if(query === "") return;

      const results = idx.search(query);
      results.forEach(result => {
        const originalCard = document.querySelector(
          '.publication-card[data-url="' + result.ref + '"]'
        );
        if(originalCard){
          const clone = originalCard.cloneNode(true);
          clone.classList.add("expanded");
          clone.style.maxWidth = "100%";
          resultsContainer.appendChild(clone);
        }
      });
    });
  });
</script>

<section class="year-wrapper">

{% assign years = "2025/2026" | split: "," %}

{% for year in years %}
  <div class="year-block">
    <div class="year-header">{{ year }}</div>
    <div class="year-content">
      <div class="publication-grid">
      {% assign sorted_publications = site.publications | sort: "date" | reverse %}
      {% for item in sorted_publications %}
        {% if item.year == year %}
          <div class="publication-card" data-url="{{ item.url }}">
            <div class="publication-card-front" onclick="toggleCard(this)">
              <img src="{{ item.image }}" alt="{{ item.title }}">
              <div class="card-overlay">
                <h3>{{ item.title }}</h3>
                <p>{{ item.date }}</p>
              </div>
            </div>
            <div class="publication-detail">
              {{ item.content }}
              <div class="button-wrapper">
                {% if item.links %}
                  {% for link in item.links %}
                    <a href="{{ link.url }}" target="_blank" class="card-btn {% if link.secondary %}secondary{% endif %}">
                      {{ link.name }}
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
