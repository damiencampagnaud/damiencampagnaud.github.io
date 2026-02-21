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

/* ================= ANNEE ================= */
.year-header {
  text-align: center;
  font-weight: bold;
  color: black;
  font-size: 22px;
  margin-bottom: 15px;
  cursor: pointer;
  padding: 18px;
  border-radius: 12px;
  background: white;
  box-shadow: 0 4px 10px rgba(0,0,0,0.08);
}

.year-content {
  display:none;
  margin-bottom:40px;
}

.year-content.open {
  display:block;
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
  flex: 1 1 100%;
  max-width:100%;
}

/* ================= FRONT ================= */
.publication-card-front {
  position:relative;
  cursor:pointer;
  border-radius:12px;
  overflow:hidden;
  box-shadow:0 4px 12px rgba(0,0,0,0.1);
  transition: transform 0.2s;
}

.publication-card-front:hover {
  transform: translateY(-3px);
}

.publication-card-front img {
  width:100%;
  height: 200px;
  object-fit: cover;
  display:block;
  transition: transform 0.3s;
}

.publication-card-front:hover img {
  transform: scale(1.05);
}

.card-overlay {
  position:absolute;
  bottom:0;
  width:100%;
  background:rgba(0,0,0,0.6);
  color:white;
  padding:10px;
  text-align:center;
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

  .publication-card-front img {
    height:180px;
  }
}
</style>

<script src="https://unpkg.com/lunr/lunr.js"></script>

<script>
fetch('/search.json')
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

function toggleYear(element) {
  const content = element.nextElementSibling;
  content.classList.toggle("open");
}

function togglePublicationCard(element) {
  const card = element.closest('.publication-card');
  document.querySelectorAll('.publication-card').forEach(c => {
    if (c !== card) c.classList.remove('expanded');
  });
  card.classList.toggle('expanded');
}
</script>

<section class="year-wrapper">

  <div class="year-block">
    <div class="year-header" onclick="toggleYear(this)">
      2025/2026
    </div>

    <div class="year-content">

      <div class="publication-grid">

        {% assign publications_sorted = site.publications | sort: 'date' | reverse %}

        {% for pub in publications_sorted %}
          {% if pub.year == "2025/2026" %}
          <div class="publication-card" data-url="{{ pub.url }}">

            <div class="publication-card-front" onclick="togglePublicationCard(this)">
              <img src="{{ pub.image }}" alt="{{ pub.title }}">
              <div class="card-overlay">
                <h3>{{ pub.title }}</h3>
              </div>
            </div>

            <div class="publication-detail">
              {{ pub.content }}
              <div class="button-wrapper">
                {% for link in pub.links %}
                  <a href="{{ link.url }}" target="_blank" class="card-btn {% if link.secondary %}secondary{% endif %}">
                    {{ link.name }}
                  </a>
                {% endfor %}
              </div>
            </div>

          </div>
          {% endif %}
        {% endfor %}

      </div>
    </div>
  </div>

</section>
