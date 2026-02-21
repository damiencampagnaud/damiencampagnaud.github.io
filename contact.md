---
layout: default
title: "Contact - Damien CAMPAGNAUD"
permalink: /contact/
---

<style>
.contact-container {
  max-width: 800px;
  margin: 60px auto;
  text-align: center;
  padding: 0 20px;
}

.contact-logo {
  width: 180px;
  height: auto;
  margin-bottom: 40px;
}

.contact-text {
  font-size: 1.2rem;
  margin-bottom: 30px;
}

.email-link {
  display: block;
  font-size: 1.3rem;
  font-weight: bold;
  margin: 15px 0;
  color: #1e73be;
  cursor: pointer;
  text-decoration: none;
  transition: 0.2s ease;
}

.email-link:hover {
  opacity: 0.8;
}

.copy-message {
  font-size: 0.9rem;
  color: green;
  display: none;
  margin-top: 5px;
}
</style>

<div class="contact-container">

  <img src="/assets/images/Logo.png" alt="Logo Damien Campagnaud" class="contact-logo">

  <div class="contact-text">
    Vous pouvez me contacter aux adresses suivantes :
  </div>

  <a class="email-link" onclick="copyEmail('damien.campagnaud@gmail.com')">
    damien.campagnaud@gmail.com
  </a>

  <a class="email-link" onclick="copyEmail('damien.campagnaud@ac-versailles.fr')">
    damien.campagnaud@ac-versailles.fr
  </a>

  <div id="copyMessage" class="copy-message">
    Adresse copiée dans le presse-papier ✓
  </div>

</div>

<script>
function copyEmail(email) {
  navigator.clipboard.writeText(email).then(function() {
    const msg = document.getElementById("copyMessage");
    msg.style.display = "block";
    setTimeout(() => {
      msg.style.display = "none";
    }, 2000);
  });
}
</script>
