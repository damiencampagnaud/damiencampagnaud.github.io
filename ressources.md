---
layout: default
title: "Ressources"
permalink: /ressources/
---

<section class="ressources-wrapper">

  <section class="search-section">
    <input type="text" id="resSearchBox" placeholder="Rechercher un Genially...">
    <div id="resSearchResults"></div>
  </section>

  <style>
    /* ================= SEARCH ================= */
    .ressources-wrapper .search-section {
      text-align:center;
      margin-bottom:40px;
    }
    .ressources-wrapper #resSearchBox {
      width:60%;
      max-width:500px;
      padding:12px;
      border-radius:8px;
      border:1px solid #ccc;
      font-size:16px;
    }
    .ressources-wrapper #resSearchResults { margin-top:30px; }

    /* ================= NIVEAUX ================= */
    .ressources-wrapper .niveau-header {
      background:white;
      padding:18px;
      border-radius:12px;
      cursor:pointer;
      margin-bottom:15px;
      box-shadow:0 4px 10px rgba(0,0,0,0.08);
    }
    .ressources-wrapper .niveau-content { display:none; margin-bottom:40px; }
    .ressources-wrapper .niveau-content.open { display:block; }

    /* ================= GRID ================= */
    .ressources-wrapper .genially-grid {
      display:flex;
      flex-wrap:wrap;
      gap:20px;
      justify-content:center;
    }
    .ressources-wrapper .genially-card {
      flex: 1 1 300px;
      max-width:350px;
      transition:0.3s;
    }
    .ressources-wrapper .genially-card.expanded {
      flex:1 1 100%;
      max-width:100%;
    }

    /* ================= FRONT ================= */
    .ressources-wrapper .genially-card-front {
      position:relative;
      cursor:pointer;
      border-radius:12px;
      overflow:hidden;
      box-shadow:0 4px 12px rgba(0,0,0,0.1);
    }
    .ressources-wrapper .genially-card-front img { width:100%; display:block; }
    .ressources-wrapper .card-overlay {
      position:absolute;
      bottom:0;
      width:100%;
      background:rgba(0,0,0,0.6);
      color:white;
      padding:10px;
    }

    /* ================= DETAIL ================= */
    .ressources-wrapper .genially-detail {
      display:none;
      background:white;
      padding:30px;
      margin-top:15px;
      border-radius:12px;
      box-shadow:0 6px 18px rgba(0,0,0,0.1);
      text-align:center;
    }
    .ressources-wrapper .genially-card.expanded .genially-detail { display:block; }

    /* ================= BUTTONS ================= */
    .ressources-wrapper .button-wrapper {
      margin-top:20px;
      display:flex;
      flex-wrap:wrap;
      justify-content:center;
      gap:15px;
    }
    .ressources-wrapper .genially-detail .button-wrapper a.card-btn {
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
    .ressources-wrapper .genially-detail .button-wrapper a.card-btn.secondary {
      background:#3b82f6;
    }
    .ressources-wrapper .genially-detail .button-wrapper a.card-btn:hover { opacity:0.9; }

    @media (max-width:768px){
      .ressources-wrapper #resSearchBox { width:90%; }
      .ressources-wrapper .genially-card { flex:1 1 100%; max-width:100%; }
    }
  </style>

  <script src="https://unpkg.com/lunr/lunr.js"></script>
  <script>
    function toggleNiveau(element) {
      const content = element.nextElementSibling;
      content.classList.toggle("open");
    }

    function toggleCardRes(element) {
      const card = element.closest('.genially-card');
      document.querySelectorAll('.ressources-wrapper .genially-card').forEach(c => {
        if(c!==card) c.classList.remove('expanded');
      });
      card.classList.toggle("expanded");
    }

    fetch('/search.json')
      .then(response => response.json())
      .then(data => {
        const idx = lunr(function() {
          this.ref('url')
          this.field('title')
          this.field('content')
          data.forEach(doc => this.add(doc))
        })

        document.getElementById('resSearchBox').addEventListener('input', function() {
          const query = this.value.trim();
          const resultsContainer = document.getElementById('resSearchResults');
          resultsContainer.innerHTML = "";
          if(query==="") return;
          const results = idx.search(query);
          results.forEach(result => {
            const originalCard = document.querySelector('.ressources-wrapper .genially-card[data-url="'+result.ref+'"]');
            if(originalCard){
              const clone = originalCard.cloneNode(true);
              clone.classList.add('expanded');
              clone.style.maxWidth="100%";
              resultsContainer.appendChild(clone);
            }
          });
        });
      })
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
                <div class="genially-card" data-url="{{ item.url }}">
                  <div class="genially-card-front" onclick="toggleCardRes(this)">
                    <img src="{{ item.image }}" alt="{{ item.title }}">
                    <div class="card-overlay">
                      <h3>{{ item.title }}</h3>
                      <p>{{ item.niveau }}</p>
                    </div>
                  </div>
                  <div class="genially-detail">
                    {{ item.content }}
                    <div class="button-wrapper">
                      {% if item.pdf_files %}
                        {% for pdf in item.pdf_files %}
                          <a href="{{ pdf.url }}" target="_blank" class="card-btn {% if forloop.index > 1 %}secondary{% endif %}">
                            📄 {{ pdf.name }}
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

</section>
