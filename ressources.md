# 📚 Ressources

<div style="text-align:center; margin-bottom:30px;">
  <input type="text" id="searchInput" placeholder="🔎 Rechercher un Genially..."
    style="width:60%; padding:12px; font-size:16px; border-radius:8px; border:1px solid #ccc;">
</div>

---

## 6ème

<div class="level-banner" onclick="toggleLevel('six')">
  ⬇️ Voir les ressources 6ème
</div>

<div id="six" class="level-content">

  <div class="card" data-title="Les débuts de l'humanité">
    <div class="card-inner" onclick="openResource(this)">
      <h3>Les débuts de l'humanité</h3>
      <p>Préhistoire – Premiers hommes</p>
    </div>

    <div class="resource-detail">
      <h2>Les débuts de l'humanité</h2>
      <p>Genially interactif pour comprendre les premiers humains.</p>

      <div class="btn-container">
        <a href="LIEN_GENIALLY" target="_blank" class="btn">
          🎮 Ouvrir le Genially
        </a>
        <a href="LIEN_PDF" target="_blank" class="btn pdf">
          📄 Fiche activité
        </a>
      </div>
    </div>
  </div>

</div>

---

## 5ème

<div class="level-banner" onclick="toggleLevel('five')">
  ⬇️ Voir les ressources 5ème
</div>

<div id="five" class="level-content"></div>

---

## 4ème

<div class="level-banner" onclick="toggleLevel('four')">
  ⬇️ Voir les ressources 4ème
</div>

<div id="four" class="level-content"></div>

---

## 3ème

<div class="level-banner" onclick="toggleLevel('three')">
  ⬇️ Voir les ressources 3ème
</div>

<div id="three" class="level-content"></div>

---

<style>

.level-banner {
  background:white;
  padding:18px;
  margin:20px 0;
  border-radius:12px;
  font-size:20px;
  font-weight:bold;
  cursor:pointer;
  box-shadow:0 4px 10px rgba(0,0,0,0.08);
  transition:0.3s;
}

.level-banner:hover {
  transform:scale(1.02);
}

.level-content {
  display:none;
  margin-bottom:30px;
}

.card {
  margin-bottom:20px;
}

.card-inner {
  background:white;
  padding:25px;
  border-radius:12px;
  box-shadow:0 4px 12px rgba(0,0,0,0.08);
  cursor:pointer;
  transition:0.3s;
  text-align:center;
}

.card-inner:hover {
  transform:translateY(-4px);
}

.resource-detail {
  display:none;
  background:white;
  margin-top:20px;
  padding:40px;
  border-radius:12px;
  box-shadow:0 8px 20px rgba(0,0,0,0.1);
  width:100%;
}

.resource-detail.active {
  display:block;
}

.btn-container {
  margin-top:25px;
  text-align:center;
}

.btn {
  display:inline-block;
  margin:10px;
  padding:12px 22px;
  border-radius:8px;
  background:#159957;
  color:white;
  text-decoration:none;
  font-weight:bold;
  transition:0.3s;
}

.btn:hover {
  background:#0e7c41;
}

.btn.pdf {
  background:#3b82f6;
}

.btn.pdf:hover {
  background:#1d4ed8;
}

</style>

<script>

function toggleLevel(id) {
  let content = document.getElementById(id);
  content.style.display =
    content.style.display === "block" ? "none" : "block";
}

function openResource(element) {
  let detail = element.nextElementSibling;

  let allDetails = document.querySelectorAll(".resource-detail");
  allDetails.forEach(d => d.classList.remove("active"));

  detail.classList.add("active");
  detail.scrollIntoView({ behavior: "smooth" });
}

document.getElementById("searchInput").addEventListener("keyup", function () {

  let filter = this.value.toLowerCase();
  let cards = document.querySelectorAll(".card");

  cards.forEach(card => {

    let title = card.getAttribute("data-title").toLowerCase();
    let level = card.closest(".level-content");

    if (title.includes(filter)) {
      card.style.display = "block";
      level.style.display = "block";
    } else {
      card.style.display = "none";
    }

  });

});

</script>
