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

/* ================= YEAR HEADER ================= */
.year-header {
  text-align:center;
  font-weight:bold;
  font-size:24px;
  color:black;
  margin-bottom:20px;
  cursor:pointer;
  background:white;
  padding:18px;
  border-radius:12px;
  box-shadow:0 4px 10px rgba(0,0,0,0.08);
}

.year-header h2 {
  margin:0;
}

/* ================= GRID ================= */
.publication-grid {
  display:flex;
  flex-direction:column;
  gap:30px;
  align-items:center;
}

.publication-card {
  display:flex;
  flex-direction:row;
  width:100%;
  max-width:900px;
  transition:0.3s;
  align-items:flex-start;
}

.publication-card.expanded {
  flex-direction:column;
  max-width:100%;
  align-items:center;
}

/* ================= FRONT ================= */
.publication-card-front {
  position:relative;
  cursor:pointer;
  border-radius:12px;
  overflow:hidden;
  box-shadow:0 4px 12px rgba(0,0,0,0.1);
  flex-shrink:0;
  width:250px;
  height:auto;
  margin-right:20px;
}

.publication-card.expanded .publication-card-front {
  width:100%;
  margin-right:0;
  height:auto;
}

.publication-card-front img {
  width:100%;
  height:auto;
  display:block;
  object-fit:cover;
}

.card-overlay {
  position:absolute;
  top:0;
  width:100%;
  height:100%;
  display:flex;
  flex-direction:column;
  justify-content:center;
  align-items:center;
  background:rgba(0,0,0,0);
  color:white;
  opacity:0;
  transition:0.3s;
  text-align:center;
  padding:10px;
}

.publication-card-front:hover .card-overlay {
  opacity:1;
  background:rgba(0,0,0,0.6);
}

.card-overlay h3 {
  font-weight:bold;
  margin:0 0 5px 0;
}

.card-overlay span {
  font-weight:normal;
  font-size:14px;
}

/* ================= DETAIL ================= */
.publication-detail {
  display:none;
  background:white;
  padding:30px;
  border-radius:12px;
  box-shadow:0 6px 18px rgba(0,0,0,0.1);
  text-align:center;
  flex:1;
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
    flex-direction:column;
    align-items:center;
  }

  .publication-card-front {
    width:100%;
    margin-right:0;
  }
}

</style>

<script src="https://unpkg.com/lunr/lunr.js"></script>

<script>

// ================= SEARCH =================
fetch('/search.json')
  .then(response => response.json())
  .then(data => {

    const idx = lunr(function () {
      this.ref('url')
      this.field('title')
      this.field('content')
      data.forEach(doc => this.add(doc))
    })

    const searchBox = document.getElementById('searchBox')
    const resultsContainer = document.getElementById('searchResults')

    searchBox.addEventListener('input', function() {

      const query = this.value.trim()
      resultsContainer.innerHTML = ""

      if(query === "") return

      const results = idx.search(query)

      results.forEach(result => {

        const originalCard = document.querySelector(
          '.publication-card[data-url="' + result.ref + '"]'
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

// ================= TOGGLE =================
function toggleNiveau(element) {
  const content = element.nextElementSibling;
  content.classList.toggle("open");
}

function toggleCard(element) {
  const card = element.closest('.publication-card');
  card.classList.toggle("expanded");

  // fermer les autres publications dans la même année
  const parentGrid = card.closest('.publication-grid');
  parentGrid.querySelectorAll('.publication-card').forEach(c => {
    if(c !== card) c.classList.remove('expanded');
  });
}

</script>

<!-- =================== PUBLICATIONS 2025/2026 =================== -->
<div class="year-block">
  <div class="year-header" onclick="toggleNiveau(this)"><h2>2025/2026</h2></div>
  <div class="niveau-content open">
    <div class="publication-grid">
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

<!-- =================== PUBLICATIONS 2026/2027 =================== -->
<div class="year-block">
  <div class="year-header" onclick="toggleNiveau(this)"><h2>2026/2027</h2></div>
  <div class="niveau-content">
    <div class="publication-grid">
      {% for item in site.publications %}
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
