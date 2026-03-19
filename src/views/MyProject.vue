<template>
  <section id="Projets">
    <h3>Ci-dessous, vous trouverez les projets réalisés dernièrement:</h3>
    <br />
    <div class="mes_projets">
      <div v-for="(project, index) in projects" :key="index" class="project">
        <figure>
          <img
            :src="project.src"
            @click="openModal(project)"
            class="project-image"
            :alt="project.title"
          />
          <figcaption>
            {{ project.title }} <br />
            {{ project.date }}
          </figcaption>
        </figure>
      </div>
      <Modal :is-open-modal="isModalOpen" :project="selectedProject" @close="closeModal" />
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'
import Modal from '@/components/Modal.vue'

const project = ref()

//Détails pour les différents projets
const projects = ref([
    {
    id: 7,
    src: '/Images_projets/Au_petit_village_homepage.png',
    title: 'Dynamiser un site web avec Angular',
    date: 'Octobre 2024',
    description: 'Réalisation d\'un site web avec Angular pour l\'entreprise Au Petit Village',
    technologies: 'Angular, TypeScript, HTML, CSS, Node.js, AngularCLI, RxJS, Express.js',
    githubLink: 'https://github.com/Ad15C/Au_Petit_Village.git'
  },

  {
    id: 8,
    src: '/Images_projets/trouve_ton_artisan_homepage.png',
    title: 'Site Trouve ton Artisan',
    date: 'Novembre 2024',
    description: 'Réalisation du site web Trouve ton Artisan avec Angular.js',
    technologies: 'Angular.js, HTML, CSS, JavaScript, Node.js, NPM, Always Data, EmailJS, Figma',
    pdfLink: '/PDF/Plateforme_Trouve_Ton_Artisan.pdf',
    githubLink: 'https://github.com/Ad15C/Artisan.git'
  },
  
  {
    id: 10,
    src: '/Images_projets/mediatheque_staff_dashboard.png',
    title: 'Moderniser le système de gestion interne d\'une médiathèque',
    date: 'Juin 2025',
    description: 'Réalisation d\'une mise à jour du système de gestion interne d\'une médiathèque avec Python et Django',
    technologies: 'HTML, CSS, Python, Django, SQLite, Pytest',
    pdfLink: '/PDF/Projet_mediatheque.pdf',
    githubLink: 'https://github.com/Ad15C/django.git'
  },

  {
    id: 11,
    src: '/Images_projets/Stubborn_sweatshirts_homepage.png',
    title: 'Réalisation d\'un site e-commerce Stubborn Sweatshirts',
    date: 'Février 2026',
    description: 'Réalisation d\'un site e-commerce pour la marque Stubborn Sweatshirts avec Symfony',
    technologies: 'HTML, CSS, JavaScript, Twig, Symfony 6, PHP, Doctrine ORM, Stripe, SwiftMailer, PHPUnit',
    pdfLink: '/PDF/Dossier_stubborn.pdf',
    githubLink: 'https://github.com/Ad15C/stubborn.git'
  },

  {
    id: 12,
    src: '/Images_projets/Knowledge_learning_homepage.png',
    title: 'Création d\'une plateforme d\'apprentissage en ligne',
    date: 'Mars 2026',
    description: 'Réalisation d\'une plateforme d\'apprentissage en ligne Knowledge Learning',
    technologies: 'HTML, CSS, Twig, Symfony AssetMapper, PHP, Symfony 8, Doctrine ORM, Doctrine Migrations, MySQL, PHPUnit, Symfony BrowserKit, Liip Test Fixtures, Dompdf',
    pdfLink: '/PDF/Dossier_Knowledge_Learning.pdf',
    githubLink: 'https://github.com/Ad15C/Knowledge_learning.git'
  },
])

//Sélection du Projet
//la valeur nulle est utilisée pour l'état initial où le modal n'est pas affiché
let selectedProject = ref(null)

let isModalOpen = ref(false)

//Ouverture du Modal
const openModal = (project) => {
  selectedProject.value = project,
  isModalOpen = true
}

//Fermeture du Modal
const closeModal = () => {
  selectedProject.value = project,
  isModalOpen = false
}

//Fermeture en cliquant en dehors du modal
const clickOutside = (event) => {
  if (isModalOpen && event.target.closest('.modal')) {
    closeModal()
  }
}
document.addEventListener('click', clickOutside)

//Fermeture en appuyant sur Escape
const handleEscape = (event) => {
  if (isModalOpen && event.key) {
    closeModal()
  }
}
document.addEventListener('keydown', handleEscape)
</script>

<style scoped>
</style>
