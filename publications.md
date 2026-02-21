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

/* ================= ANNÉES ================= */

.year-header {
  background:white;
  padding:18px;
  border-radius:12px;
  cursor:pointer;
  margin-bottom:15px;
  box-shadow:0 4px 10px rgba(0,0,0,0.08);
}

.year-content {
  display:none;
  margin-bottom:40px;
}

.year-content.open {
  display:block;
}

/* ================= THREAD ================= */

.publication-thread {
  display:flex;
  flex-direction:column;
  gap:15px;
}

.publication-card {
  background:white;
  padding:20px;
  border-radius:12px;
  box-shadow:0 4px 12px rgba(0,0,0,0.08);
  cursor:pointer;
  transition:0.3s;
}

.publication-card.expanded {
  background:#f9f9f9;
}

.publication-date {
  font-size:14px;
  color:#555;
  margin-bottom:8px;
}

.publication-title {
  font-weight:bold;
  margin-bottom:10px;
}

.publication-content {
  display:none;
  margin-top:10px;
}

.publication-card.expanded .publication-content {
  display:block;
}

/* ================= MOBILE ================= */

@media (max-width:768px) {
  #searchBox {
    width:90%;
  }
}

</style>

<script src="https://unpkg.com/lunr/lunr.js"></script>

<script>

// ==== Search functionality ====
fetch('/search_publications.json')
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
          resultsContainer.appendChild(clone)

        }

      })

    })

  })

function toggleYear(element) {
  const content = element.nextElementSibling;
  content.classList.toggle("open");
}

function togglePublication(element) {
  const card = element.closest('.publication-card');
  card.classList.toggle("expanded");
}

</script>

<section class="year-wrapper">

{% assign years = "2025/2026,2024/2025,2023/2024" | split: "," %}

{% for year in years %}

  <div class="year-block">

    <div class="year-header" onclick="toggleYear(this)">
      <h2>{{ year }}</h2>
    </div>

    <div class="year-content">

      <div class="publication-thread">

      {% assign pubs = site.publications | where: "year", year | sort: "date" | reverse %}

      {% for pub in pubs %}

        <div class="publication-card" data-url="{{ pub.url }}">

          <div class="publication-date">{{ pub.date | date: "%d/%m/%Y" }}</div>
          <div class="publication-title">{{ pub.title }}</div>

          <div class="publication-content">
            {{ pub.content }}
            {% if pub.links %}
              <div class="button-wrapper" style="margin-top:10px;">
              {% for link in pub.links %}
                <a href="{{ link.url }}" target="_blank" class="card-btn">{{ link.name }}</a>
              {% endfor %}
              </div>
            {% endif %}
          </div>

        </div>

      {% endfor %}

      </div>

    </div>

  </div>

{% endfor %}

</section>
