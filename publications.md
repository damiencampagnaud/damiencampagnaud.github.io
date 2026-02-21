---
layout: default
title: "Publications"
permalink: /publications/
---

<section class="search-section">
  <input type="text" id="pubSearchBox" placeholder="Rechercher une publication...">
  <div id="pubSearchResults"></div>
</section>

<style>

/* ================= SEARCH ================= */

.search-section {
  text-align:center;
  margin-bottom:40px;
}

#pubSearchBox {
  width:60%;
  max-width:500px;
  padding:12px;
  border-radius:8px;
  border:1px solid #ccc;
  font-size:16px;
}

#pubSearchResults {
  margin-top:30px;
}

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

/* ================= GRID ================= */

.pub-grid {
  display:flex;
  flex-wrap:wrap;
  gap:20px;
  justify-content:center;
}

.pub-card {
  flex: 1 1 300px;
  max-width:350px;
  transition:0.3s;
}

.pub-card.expanded {
  flex: 1 1 100%;
  max-width:100%;
}

/* ================= FRONT ================= */

.pub-card-front {
  position:relative;
  cursor:pointer;
  border-radius:12px;
  overflow:hidden;
  box-shadow:0 4px 12px rgba(0,0,0,0.1);
}

.pub-card-front img {
  width:100%;
  display:block;
}

.pub-overlay {
  position:absolute;
  bottom:0;
  width:100%;
  background:rgba(0,0,0,0.6);
  color:white;
  padding:10px;
}

/* ================= DETAIL ================= */

.pub-detail {
  display:none;
  background:white;
  padding:30px;
  margin-top:15px;
  border-radius:12px;
  box-shadow:0 6px 18px rgba(0,0,0,0.1);
  text-align:center;
}

.pub-card.expanded .pub-detail {
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

.pub-detail .button-wrapper a.card-btn {
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

.pub-detail .button-wrapper a.card-btn.secondary {
  background:#3b82f6;
}

.pub-detail .button-wrapper a.card-btn:hover {
  opacity:0.9;
}

/* ================= MOBILE ================= */

@media (max-width:768px) {
  #pubSearchBox {
    width:90%;
  }

  .pub-card {
    flex:1 1 100%;
    max-width:100%;
  }
}

</style>

<script src="https://unpkg.com/lunr/lunr.js"></script>
<script>

fetch('/publications.json')
  .then(response => response.json())
  .then(data => {

    const idx = lunr(function () {
      this.ref('url')
      this.field('title')
      this.field('content')
      data.forEach(doc => this.add(doc))
    })

    document.getElementById('pubSearchBox').addEventListener('input', function() {

      const query = this.value.trim()
      const resultsContainer = document.getElementById('pubSearchResults')
      resultsContainer.innerHTML = ""

      if(query === "") return

      const results = idx.search(query)

      results.forEach(result => {

        const originalCard = document.querySelector(
          '.pub-card[data-url="' + result.ref + '"]'
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

function toggleCard(element) {
  const card = element.closest('.pub-card');

  document.querySelectorAll('.pub-card').forEach(c => {
    if (c !== card) {
      c.classList.remove('expanded');
    }
  });

  card.classList.toggle("expanded");
}

</script>

<section class="year-wrapper">

{% assign year = "2025/2026" %}

<div class="year-block">

  <div class="year-header" onclick="toggleYear(this)">
    <h2>{{ year }}</h2>
  </div>

  <div class="year-content open">

    <div class="pub-grid">

    {% assign pubs = site.publications | where: "year", year | sort: "date" | reverse %}

    {% for pub in pubs %}

      <div class="pub-card" data-url="{{ pub.url }}">

        <div class="pub-card-front" onclick="toggleCard(this)">
          {% if pub.image %}
            <img src="{{ pub.image }}" alt="{{ pub.title }}">
          {% else %}
            <img src="/assets/images/default-publication.png" alt="{{ pub.title }}">
          {% endif %}
          <div class="pub-overlay">
            <h3>{{ pub.title }}</h3>
            <p>{{ pub.date | date: "%d %B %Y" }}</p>
          </div>
        </div>

        <div class="pub-detail">
          <div>{{ pub.content }}</div>
          {% if pub.links %}
          <div class="button-wrapper">
            {% for link in pub.links %}
              <a href="{{ link.url }}" target="_blank" class="card-btn {% if link.secondary %}secondary{% endif %}">{{ link.name }}</a>
            {% endfor %}
          </div>
          {% endif %}
        </div>

      </div>

    {% endfor %}

    </div>

  </div>

</div>

</section>
