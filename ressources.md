---
layout: default
title: "Ressources"
permalink: /ressources/
---

<section class="search-section">
  <input type="text" id="searchBox" placeholder="Rechercher un Genially...">
  <div id="searchResults"></div>
</section>

<script src="https://unpkg.com/lunr/lunr.js"></script>
<script>
fetch('/search.json')
  .then(response => response.json())
  .then(data => {

    const idx = lunr(function () {
      this.ref('url')
      this.field('title')
      this.field('niveau')
      this.field('content')

      data.forEach(doc => this.add(doc))
    })

    document.getElementById('searchBox').addEventListener('input', function() {
      const results = idx.search(this.value)
      let output = ""

      results.forEach(result => {
        const match = data.find(d => d.url === result.ref)
        output += `
          <div class="search-card">
            <h3>${match.title}</h3>
            <p>${match.niveau}</p>
            <a href="${match.genially_url}" target="_blank" class="card-btn">🎮 Ouvrir</a>
            <a href="${match.url}" class="card-btn secondary">📄 Voir la ressource</a>
          </div>
        `
      })

      document.getElementById('searchResults').innerHTML = output
    })
  })
</script>

<script>
function toggleNiveau(element) {
  const content = element.nextElementSibling;
  content.classList.toggle("open");
}

function toggleCard(element) {
  const card = element.parentElement;
  card.classList.toggle("expanded");
}
</script>
