<template>
  <section id="Projets">
    <h3>Projets réalisés</h3>
    <br />
    <div class="mes_projets">
      <div v-for="project in projects" :key="project.id" class="project">
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

      <Modal
        v-if="selectedProject"
        :isOpenModal="isModalOpen"
        :project="selectedProject"
        @close="closeModal"
      />
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import Modal from '@/components/Modal.vue'

const projects = ref([
  {
    id: 1,
    src: '/Images_projets/cv.png',
    title: 'CV en ligne',
    date: 'Décembre 2023',
    description: "Réalisation d’un curriculum vitae",
    technologies: 'HTML, CSS, GitHub, VS Code',
    githubLink: 'https://github.com/Ad15C/Mon_cv.git'
  },
  {
    id: 2,
    src: '/Images_projets/cahier_des_charges.png',
    title: 'Cahier des charges – La Socketterie',
    date: 'Décembre 2023',
    description: "Rédaction d’un cahier des charges pour La Socketterie dans le cadre d’une refonte du site internet et d’une analyse concurrentielle",
    technologies: 'Word',
    pdfLink: '/PDF/Cahier_des_charges.pdf'
  },
  {
    id: 3,
    src: '/Images_projets/commentaire_dynamique.png',
    title: 'Système de commentaires dynamiques',
    date: 'Février 2024',
    description: "Développement d’un système de commentaires dynamiques en JavaScript",
    technologies: 'HTML, CSS, GitHub, VS Code, JavaScript',
    githubLink: 'https://github.com/Ad15C/mon_commentaire.git'
  },
  {
    id: 4,
    src: '/Images_projets/prototype_desktop_probeats.png',
    title: 'Maquette UI – Probeats',
    date: 'Juillet 2024',
    description: "Conception d’une maquette d’interface utilisateur avec Figma",
    technologies: 'Figma',
    githubLink: 'https://github.com/Ad15C/Figma.git'
  },
  {
    id: 5,
    src: '/Images_projets/cv_react_homepage.jpg',
    title: 'CV en ligne avec React',
    date: 'Août 2024',
    description: "Réalisation d'un CV en ligne avec React.js",
    technologies: 'React.js, HTML, CSS, Sass, Bootstrap, JavaScript, Vite, GitHub, VS Code',
    githubLink: 'https://github.com/Ad15C/mon_cv_react.git'
  },
  {
    id: 6,
    src: '/Images_projets/site_wordpress_homepage.png',
    title: 'Site vitrine WordPress – La Vie des Plantes',
    date: 'Août 2024',
    description: "Création d’un site web avec WordPress pour l’entreprise La Vie des Plantes",
    technologies: 'WordPress, HTML, CSS, JavaScript, Elementor,Yoast SEO, Always Data, GitHub, VS Code',
    githubLink: 'https://github.com/Ad15C/La-Vie-Des-Plantes.git'
  },
  {
    id: 7,
    src: '/Images_projets/Au_petit_village_homepage.png',
    title: 'Site web dynamique – Au Petit Village',
    date: 'Octobre 2024',
    description: "Réalisation d'un site web avec Angular pour l’entreprise Au Petit Village",
    technologies: 'Angular, TypeScript, HTML, CSS, Node.js, Angular CLI, RxJS, Express.js, MongoDB, Mongoose, GitHub, VS Code',
    githubLink: 'https://github.com/Ad15C/Au_Petit_Village.git'
  },
  {
    id: 8,
    src: '/Images_projets/trouve_ton_artisan_homepage.png',
    title: 'Plateforme web – Trouve ton Artisan',
    date: 'Novembre 2024',
    description: 'Réalisation du site web Trouve ton Artisan avec Angular',
    technologies: 'Angular, HTML, CSS, JavaScript, Node.js, NPM, Always Data, EmailJS, Figma, GitHub, VS Code',
    pdfLink: '/PDF/Plateforme_Trouve_Ton_Artisan.pdf',
    githubLink: 'https://github.com/Ad15C/Artisan.git'
  },
  {
    id: 9,
    src: '/Images_projets/Tifosi.png',
    title: 'Base de données – Tifosi',
    date: 'Janvier 2025',
    description: "Réalisation d’une base de données pour le site Tifosi avec MySQL",
    technologies: 'MySQL, MySQL Workbench, GitHub, VS Code',
    githubLink: 'https://github.com/Ad15C/tifosi.git'
  },
  {
    id: 10,
    src: '/Images_projets/mediatheque_staff_dashboard.png',
    title: 'Application de gestion – Médiathèque',
    date: 'Juin 2025',
    description: "Réalisation d’une mise à jour du système de gestion interne d’une médiathèque avec Python et Django",
    technologies: 'HTML, CSS, Python, Django, SQLite, Pytest, GitHub, VS Code',
    pdfLink: '/PDF/Projet_mediatheque.pdf',
    githubLink: 'https://github.com/Ad15C/django.git'
  },
  {
    id: 11,
    src: '/Images_projets/Stubborn_sweatshirts_homepage.png',
    title: 'Site e-commerce – Stubborn Sweatshirts',
    date: 'Février 2026',
    description: "Réalisation d’un site e-commerce pour la marque Stubborn Sweatshirts avec Symfony",
    technologies: 'HTML, CSS, JavaScript, Twig, Symfony 6, PHP, Doctrine ORM, Stripe, SwiftMailer, PHPUnit, GitHub, VS Code',
    pdfLink: '/PDF/Dossier_stubborn.pdf',
    githubLink: 'https://github.com/Ad15C/stubborn.git'
  },
  {
    id: 12,
    src: '/Images_projets/Knowledge_learning_homepage.png',
    title: 'Plateforme e-learning – Knowledge Learning',
    date: 'Mars 2026',
    description: "Réalisation d’une plateforme d’apprentissage en ligne Knowledge Learning",
    technologies: 'HTML, CSS, Twig, Symfony AssetMapper, PHP, Symfony 8, Doctrine ORM, Doctrine Migrations, MySQL, PHPUnit, Symfony BrowserKit, Liip Test Fixtures, Dompdf, GitHub, VS Code',
    pdfLink: '/PDF/Dossier_Knowledge_Learning.pdf',
    githubLink: 'https://github.com/Ad15C/Knowledge_learning.git'
  }
])

const selectedProject = ref(null)
const isModalOpen = ref(false)

const openModal = (project) => {
  selectedProject.value = project
  isModalOpen.value = true
}

const closeModal = () => {
  selectedProject.value = null
  isModalOpen.value = false
}

const clickOutside = (event) => {
  if (isModalOpen.value && event.target.classList.contains('modal')) {
    closeModal()
  }
}

const handleEscape = (event) => {
  if (isModalOpen.value && event.key === 'Escape') {
    closeModal()
  }
}

onMounted(() => {
  document.addEventListener('click', clickOutside)
  document.addEventListener('keydown', handleEscape)
})

onUnmounted(() => {
  document.removeEventListener('click', clickOutside)
  document.removeEventListener('keydown', handleEscape)
})
</script>

<style scoped>
</style>