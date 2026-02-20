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

.genially-card {
  width:100%;
  margin-bottom:40px;
}

.genially-card-front {
  position:relative;
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
  padding:15px;
}

.genially-detail {
  background:white;
  padding:30px;
  border-radius:12px;
  margin-top:15px;
  box-shadow:0 6px 18px rgba(0,0,0,0.1);
}

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

      if(query === ""){
        document.getElementById('searchResults').innerHTML = ""
        return
      }

      const results = idx.search(query)

      let output = ""

      results.forEach(result => {

        const match = data.find(d => d.url === result.ref)

        output += `
          <div class="genially-card">
            <div class="genially-card-front">
              <img src="${match.image}" alt="${match.title}">
              <div class="card-overlay">
                <h2>${match.title}</h2>
                <p>${match.niveau}</p>
              </div>
            </div>

            <div class="genially-detail">
              <p>${match.content}</p>

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
</script>
