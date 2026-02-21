---
layout: default
title: "Publications"
permalink: /publications/
---

<section class="publications-wrapper">

  <section class="search-section">
    <input type="text" id="pubSearchBox" placeholder="Rechercher une publication...">
    <div id="pubSearchResults"></div>
  </section>

  <style>
    /* ================= SEARCH ================= */
    .publications-wrapper .search-section { text-align:center; margin-bottom:40px; }
    .publications-wrapper #pubSearchBox { width:60%; max-width:500px; padding:12px; border-radius:8px; border:1px solid #ccc; font-size:16px; }
    .publications-wrapper #pubSearchResults { margin-top:30px; }

    /* ================= YEAR ================= */
    .publications-wrapper .year-header { text-align:center; font-weight:bold; font-size:24px; color:black; margin-bottom:20px; }

    /* ================= GRID ================= */
    .publications-wrapper .publication-grid { display:flex; flex-wrap:wrap; gap:20px; justify-content:center; }
    .publications-wrapper .publication-card { flex: 1 1 300px; max-width:350px; transition:0.3s; }
    .publications-wrapper .publication-card.expanded { flex:1 1 100%; max-width:100%; }

    /* ================= FRONT ================= */
    .publications-wrapper .publication-card-front { position:relative; cursor:pointer; border-radius:12px; overflow:hidden; box-shadow:0 4px 12px rgba(0,0,0,0.1); transition:0.3s; }
    .publications-wrapper .publication-card-front img { width:100%; display:block; }
    .publications-wrapper .card-overlay { position:absolute; bottom:0; width:100%; background:rgba(0,0,0,0.6); color:white; padding:10px; display:flex; justify-content:space-between; align-items:center; font-size:14px; opacity:0; transition:0.3s; }
    .publications-wrapper .publication-card-front:hover { filter: grayscale(60%); }
    .publications-wrapper .publication-card-front:hover .card-overlay { opacity:1; }

    /* ================= DETAIL ================= */
    .publications-wrapper .publication-detail { display:none; background:white; padding:30px; margin-top:15px; border-radius:12px; box-shadow:0 6px 18px rgba(0,0,0,0.1); text-align:center; }
    .publications-wrapper .publication-card.expanded .publication-detail { display:block; }

    /* ================= BUTTONS ================= */
    .publications-wrapper .button-wrapper { margin-top:20px; display:flex; flex-wrap:wrap; justify-content:center; gap:15px; }
    .publications-wrapper .publication-detail .button-wrapper a.card-btn { all: unset; display:inline-flex; align-items:center; justify-content:center; height:44px; padding:0 22px; border-radius:8px; background:#159957; color:white; font-weight:bold; font-size:15px; text-decoration:none; cursor:pointer; box-sizing:border-box; }
    .publications-wrapper .publication-detail .button-wrapper a.card-btn.secondary { background:#3b82f6; }
    .publications-wrapper .publication-detail .button-wrapper a.card-btn:hover { opacity:0.9; }

    @media (max-width:768px){
      .publications-wrapper #pubSearchBox { width:90%; }
      .publications-wrapper .publication-card { flex:1 1 100%; max-width:100%; }
    }
  </style>

  <script src="https://unpkg.com/lunr/lunr.js"></script>
  <script>
    function togglePublicationCard(element) {
      const card = element.closest('.publication-card');
      document.querySelectorAll('.publications-wrapper .publication-card').forEach(c => {
        if(c!==card) c.classList.remove('expanded');
      });
      card.classList.toggle("expanded");
    }

    const pubSearchBox = document.getElementById('pubSearchBox');
    if(pubSearchBox){
      fetch('/search.json')
        .then(response => response.json())
        .then(data => {
          const idx = lunr(function(){
            this.ref('url')
            this.field('title')
            this.field('content')
            data.forEach(doc => this.add(doc))
          });

          pubSearchBox.addEventListener('input', function(){
            const query = this.value.trim();
            const resultsContainer = document.getElementById('pubSearchResults');
            resultsContainer.innerHTML="";
            if(query==="") return;
            const results = idx.search(query);
            results.forEach(result=>{
              const originalCard = document.querySelector('.publications-wrapper .publication-card[data-url="'+result.ref+'"]');
              if(originalCard){
                const clone = originalCard.cloneNode(true);
                clone.classList.add('expanded');
                clone.style.maxWidth="100%";
                resultsContainer.appendChild(clone);
              }
            });
          });
        })
    }
  </script>

  {% assign sorted_publications = site.publications | sort:"date" | reverse %}

  <div class="year-block">
    <div class="year-header">2025/2026</div>
    <div class="publication-grid">
      {% for item in sorted_publications %}
        {% if item.year=="2025/2026" %}
          <div class="publication-card" data-url="{{ item.url }}">
            <div class="publication-card-front" onclick="togglePublicationCard(this)">
              <img src="{{ item.image }}" alt="{{ item.title }}">
              <div class="card-overlay">
                <span>{{ item.title }}</span>
                <span>{{ item.date }}</span>
              </div>
            </div>
            <div class="publication-detail">
              {{ item.content }}
              <div class="button-wrapper">
                {% if item.links %}
                  {% for link in item.links %}
                    <a href="{{ link.url }}" target="_blank" class="card-btn {% if forloop.index>1 %}secondary{% endif %}">
                      📄 {{ link.name }}
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

</section>
