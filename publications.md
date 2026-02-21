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
.search-section { text-align:center; margin-bottom:40px; }
#searchBox { width:60%; max-width:500px; padding:12px; border-radius:8px; border:1px solid #ccc; font-size:16px; }
#searchResults { margin-top:30px; }

/* ================= YEAR TOGGLE ================= */
.year-toggle { background:white; padding:18px; margin:40px 0 20px 0; border-radius:12px; box-shadow:0 4px 12px rgba(0,0,0,0.08); cursor:pointer; text-align:center; font-weight:bold; font-size:24px; color:black; transition:0.3s; }
.year-toggle:hover { background:#f5f5f5; }
.year-content { display:none; }
.year-block.expanded .year-content { display:block; }

/* ================= PUBLICATION LAYOUT ================= */
.publication-card { display:flex; gap:25px; align-items:center; width:100%; margin-bottom:40px; transition:0.3s; }
.publication-card-front { width:40%; min-width:280px; position:relative; cursor:pointer; border-radius:12px; overflow:hidden; box-shadow:0 4px 12px rgba(0,0,0,0.1); }
.publication-card-front img { width:100%; height:auto; display:block; }
.publication-detail { width:60%; background:white; padding:30px; border-radius:12px; box-shadow:0 6px 18px rgba(0,0,0,0.1); text-align:center; }
.publication-card.collapsed { display:block; }
.publication-card.collapsed .publication-card-front { width:100%; height:250px; }
.publication-card.collapsed .publication-card-front img { height:100%; object-fit:cover; }
.publication-card.collapsed .publication-detail { display:none; }

/* ================= OVERLAY ================= */
.card-overlay { position:absolute; top:0; width:100%; height:100%; display:flex; flex-direction:column; justify-content:center; align-items:center; background:rgba(0,0,0,0); color:white; opacity:0; transition:0.3s; text-align:center; padding:10px; }
.publication-card-front:hover .card-overlay { opacity:1; background:rgba(0,0,0,0.6); }
.card-overlay h3 { font-weight:bold; margin:0 0 5px 0; }
.card-overlay span { font-weight:normal; font-size:14px; }

/* ================= BUTTONS ================= */
.button-wrapper { margin-top:20px; display:flex; flex-wrap:wrap; justify-content:center; gap:15px; }
.publication-detail .button-wrapper a.card-btn { all: unset; display:inline-flex; align-items:center; justify-content:center; height:44px; padding:0 22px; border-radius:8px; background:#159957; color:white; font-weight:bold; font-size:15px; text-decoration:none; cursor:pointer; box-sizing:border-box; }
.publication-detail .button-wrapper a.card-btn.secondary { background:#3b82f6; }
.publication-detail .button-wrapper a.card-btn:hover { opacity:0.9; }

/* ================= MOBILE ================= */
@media (max-width:768px) {
  #searchBox { width:90%; }
  .publication-card { display:block; }
  .publication-card-front, .publication-detail { width:100%; }
}
</style>

<script src="https://unpkg.com/lunr/lunr.js"></script>
<script>
/* ================= TOGGLES ================= */
function toggleCard(element) {
  const card = element.closest('.publication-card');
  card.classList.toggle('collapsed');
}
function toggleYear(element) {
  const block = element.closest('.year-block');
  block.classList.toggle('expanded');
}

/* ================= SEARCH ================= */
const searchBox = document.getElementById('searchBox');
if(searchBox){
  fetch('/search.json')
    .then(response => response.json())
    .then(data => {
      const idx = lunr(function () {
        this.ref('url');
        this.field('title');
        this.field('content');
        data.forEach(doc => this.add(doc));
      });

      searchBox.addEventListener('input', function() {
        const query = this.value.trim();
        const allCards = document.querySelectorAll('.publication-card');
        const allYears = document.querySelectorAll('.year-block');

        if(query === ""){
          // afficher tout
          allCards.forEach(c => c.style.display = 'flex');
          return;
        }

        // masquer tout
        allCards.forEach(c => c.style.display = 'none');

        // rechercher
        const results = idx.search(query + "*");
        results.forEach(r => {
          const card = document.querySelector('.publication-card[data-url="' + r.ref + '"]');
          if(card){
            card.style.display = 'flex';
            const yearBlock = card.closest('.year-block');
            if(yearBlock) yearBlock.classList.add('expanded');
          }
        });
      });
    });
}
</script>

<!-- ================= PUBLICATIONS 2025/2026 ================= -->
<div class="year-block expanded">
  <div class="year-toggle" onclick="toggleYear(this)">2025/2026</div>
  <div class="year-content">
    {% assign sorted_publications = site.publications | sort: "date" | reverse %}
    {% for item in sorted_publications %}
      {% if item.year == "2025/2026" %}
        <div class="publication-card" data-url="{{ item.url }}">
          <div class="publication-card-front" onclick="toggleCard(this)">
            <img src="{{ item.image }}" alt="{{ item.title }}">
            <div class="card-overlay">
              <h3>{{ item.title }}</h3>
              <span>{{ item.date | date: "%d/%m/%Y" }}</span>
            </div>
          </div>
          <div class="publication-detail">
            {{ item.content }}
            <div class="button-wrapper">
              {% if item.links %}
                {% for link in item.links %}
                  <a href="{{ link.url }}" target="_blank" class="card-btn {% if link.secondary %}secondary{% endif %}">{{ link.name }}</a>
                {% endfor %}
              {% endif %}
            </div>
          </div>
        </div>
      {% endif %}
    {% endfor %}
  </div>
</div>

<!-- ================= PUBLICATIONS 2026/2027 ================= -->
<div class="year-block">
  <div class="year-toggle" onclick="toggleYear(this)">2026/2027</div>
  <div class="year-content">
    {% for item in sorted_publications %}
      {% if item.year == "2026/2027" %}
        <div class="publication-card" data-url="{{ item.url }}">
          <div class="publication-card-front" onclick="toggleCard(this)">
            <img src="{{ item.image }}" alt="{{ item.title }}">
            <div class="card-overlay">
              <h3>{{ item.title }}</h3>
              <span>{{ item.date | date: "%d/%m/%Y" }}</span>
            </div>
          </div>
          <div class="publication-detail">
            {{ item.content }}
            <div class="button-wrapper">
              {% if item.links %}
                {% for link in item.links %}
                  <a href="{{ link.url }}" target="_blank" class="card-btn {% if link.secondary %}secondary{% endif %}">{{ link.name }}</a>
                {% endfor %}
              {% endif %}
            </div>
          </div>
        </div>
      {% endif %}
    {% endfor %}
  </div>
</div>
