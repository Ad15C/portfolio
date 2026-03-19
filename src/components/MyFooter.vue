<template>
  <footer>
    <!-- Flèche retour haut -->
    <div id="goBackUp">
      <a href="#top" aria-label="Remonter en haut">
        <img src="../assets/favicon/fleche.png" alt="Remonter en haut" />
      </a>
    </div>

    <!-- Réseaux -->
    <div class="Networks">
      <a
        href="https://www.linkedin.com/in/adelinecanon"
        target="_blank"
        rel="noopener noreferrer"
      >
        <img src="../assets/logos/logo_linkedin.png" alt="LinkedIn" />
      </a>

      <a
        href="https://github.com/Ad15C"
        target="_blank"
        rel="noopener noreferrer"
      >
        <img src="../assets/logos/signe-github.png" alt="GitHub" />
      </a>
    </div>

    <!-- Copyright -->
    <div class="MaJ">
      <p>
        © {{ new Date().getFullYear() }} Adeline Canon — Tous droits réservés
      </p>
    </div>
  </footer>
</template>