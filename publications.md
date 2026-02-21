---
layout: default
title: "Publications"
permalink: /publications/
---

<section class="search-section">
  <input type="text" id="searchBoxPublication" placeholder="Rechercher une publication...">
  <div id="searchResultsPublication"></div>
</section>

<style>

/* ================= SEARCH ================= */

.search-section {
  text-align:center;
  margin-bottom:40px;
}

#searchBoxPublication {
  width:60%;
  max-width:500px;
  padding:12px;
  border-radius:8px;
  border:1px solid #ccc;
  font-size:16px;
}

#searchResultsPublication {
  margin-top:30px;
}

/* ================= ANNEES ================= */

.niveau-header {
  background:white;
  padding:18px;
  border-radius:12px;
  cursor:pointer;
  margin-bottom:15px;
  box-shadow:0 4px 10px rgba(0,0,0,0.08);
  text-align:center;
}

.niveau-header h2 {
  margin:0;
  font-weight:bold;
  color:black;
}

.niveau-content {
  display:none;
  margin-bottom:40px;
}

.niveau-content.open {
  display:block;
}

/* ================= GRID ================= */

.publication-grid {
  display:flex;
  flex-direction:column;
  gap:30px;
  align-items:center;
}

.publication-card {
  width:100%;
  max-width:900px;
  transition:0.3s;
}

.publication-card.expanded {
  max-width:100%;
}

/* ================= FRONT ================= */

.publication-front {
  position:relative;
  cursor:pointer;
  border-radius:12px;
  overflow:hidden;
  box-shadow:0 4px 12px rgba(0,0,0,0.1);
  text-align:center;
}

.publication-front img {
  max-width:100%;
  height:auto;
  display:block;
  margin:auto;
  transition:0.3s;
}

.publication-card:not(.expanded) .publication-front img {
  width:100%;
  height:250px;
  object-fit:cover;
}

.publication-overlay {
  position:absolute;
  bottom:0;
  width:100%;
  background:rgba(0,0,0,0.6);
  color:white;
  padding:15px;
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

</style>

<script src="https://unpkg.com/lunr/lunr.js"></script>

<script>

fetch('/search.json')
  .then(response => response.json())
  .then(data => {

    const publicationData = data.filter(doc => doc.collection === "publications")

    const idx = lunr(function () {
      this.ref('url')
      this.field('title')
      this.field('content')
      publicationData.forEach(doc => this.add(doc))
    })

    document.getElementById('searchBoxPublication').addEventListener('input', function() {

      const query = this.value.trim()
      const resultsContainer = document.getElementById('searchResultsPublication')
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

function toggleNiveau(element) {
  const content = element.nextElementSibling;
  content.classList.toggle("open");
}

function togglePublication(element) {

  const card = element.closest('.publication-card');

  document.querySelectorAll('.publication-card').forEach(c => {
    if (c !== card) {
      c.classList.remove('expanded');
    }
  });

  card.classList.toggle("expanded");

}

</script>

<section class="niveau-wrapper">

{% assign annees = site.publications | group_by_exp:"item","item.date | date: '%Y'" | sort: "name" | reverse %}

{% for annee in annees %}

  <div class="niveau-block">

    <div class="niveau-header" onclick="toggleNiveau(this)">
      <h2>{{ annee.name }}</h2>
    </div>

    <div class="niveau-content {% if forloop.first %}open{% endif %}">

      <div class="publication-grid">

      {% assign sorted_posts = annee.items | sort: "date" | reverse %}

      {% for item in sorted_posts %}

        <div class="publication-card" data-url="{{ item.url }}">

          <div class="publication-front" onclick="togglePublication(this)">
            <img src="{{ item.image }}" alt="{{ item.title }}">
            <div class="publication-overlay">
              <h3>{{ item.title }}</h3>
              <p>{{ item.date | date: "%d/%m/%Y" }}</p>
            </div>
          </div>

          <div class="publication-detail">
            {{ item.content }}
          </div>

        </div>

      {% endfor %}

      </div>

    </div>

  </div>

{% endfor %}

</section>
