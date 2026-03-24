<template>
  <!-- Section À propos -->
  <section id="a-propos">
    <div class="about-content">
      <h2>À propos</h2>

      <p>
        Développeuse web full stack en formation, je conçois des applications web
        modernes et performantes, du frontend au backend.
      </p>

      <p>
        Grâce à mes compétences en HTML, CSS, JavaScript, Angular, React,
        Symfony et Django, je suis capable de développer des projets complets
        et adaptés aux besoins.
      </p>

      <p>
        J’accorde une attention particulière à l’expérience utilisateur, afin de
        proposer des interfaces claires, efficaces et accessibles.
      </p>

      <p>
        Que ce soit pour créer un site web ou développer une application,
        je peux vous accompagner dans la réalisation de votre projet.
      </p>

      <p>
        Je suis actuellement à la recherche d’opportunités pour continuer à évoluer
        et mettre mes compétences en pratique.
      </p>
    </div>
  </section>

  <!-- Section Projets -->
  <section id="Projets" class="projects-section">
    <h3 class="projects-title">Découvrez une sélection de projets récents :</h3>
    <div class="mes_projets">
      <div v-for="project in projects" :key="project.id" class="project">
        <figure>
          <img
            :src="project.src"
            :alt="project.title"
            class="project-image"
            @click="openModal(project)"
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

  <!-- Formulaire de contact -->
  <section class="contact-section">
    <div class="contact-wrapper">
      <form id="myForm" @submit.prevent="sendForm">
        <h3 class="contact-title">Contact</h3>
        <p class="contact-intro">Une question ou un projet ? Écrivez-moi.</p>

        <label for="lastName">Nom</label>
        <input type="text" id="lastName" v-model.trim="user.lastName" required />

        <label for="firstName">Prénom</label>
        <input type="text" id="firstName" v-model.trim="user.firstName" required />

        <label for="yourMail">Email</label>
        <input type="email" id="yourMail" v-model.trim="user.yourMail" required />

        <label for="yourMessages">Message</label>
        <textarea id="yourMessages" v-model.trim="user.yourMessages" required></textarea>

        <button type="submit">Envoyer</button>
      </form>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import Modal from '@/components/Modal.vue'
import emailjs from '@emailjs/browser'

const projects = ref([
  {
    id: 10,
    src: '/Images_projets/mediatheque_staff_dashboard.png',
    title: 'Application de gestion – Médiathèque',
    date: 'Juin 2025',
    description: "Réalisation d’une mise à jour du système de gestion interne d’une médiathèque avec Python et Django",
    technologies: 'HTML, CSS, Python, Django, SQLite, Pytest, GitHub, VS Code',
    pdfLink: '/PDF/Projet_mediatheque.pdf',
    githubLink: 'https://github.com/Ad15C/mediatheque-django.git'
  },
  {
    id: 11,
    src: '/Images_projets/Stubborn_sweatshirts_homepage.png',
    title: 'Site e-commerce – Stubborn Sweatshirts',
    date: 'Février 2026',
    description: "Réalisation d’un site e-commerce pour la marque Stubborn Sweatshirts avec Symfony",
    technologies: 'HTML, CSS, JavaScript, Twig, Symfony 6, PHP, Doctrine ORM, Stripe, SwiftMailer, PHPUnit, GitHub, VS Code',
    pdfLink: '/PDF/Dossier_stubborn.pdf',
    githubLink: 'https://github.com/Ad15C/stubborn-ecommerce-symfony.git'
  },
  {
    id: 12,
    src: '/Images_projets/Knowledge_learning_homepage.png',
    title: 'Plateforme e-learning – Knowledge Learning',
    date: 'Mars 2026',
    description: "Réalisation d’une plateforme d’apprentissage en ligne Knowledge Learning",
    technologies: 'HTML, CSS, Twig, Symfony AssetMapper, PHP, Symfony 8, Doctrine ORM, Doctrine Migrations, MySQL, PHPUnit, Symfony BrowserKit, Liip Test Fixtures, Dompdf, GitHub, VS Code',
    pdfLink: '/PDF/Dossier_Knowledge_Learning.pdf',
    githubLink: 'https://github.com/Ad15C/knowledge-learning.git'
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

const user = ref({
  lastName: '',
  firstName: '',
  yourMail: '',
  yourMessages: ''
})

function sendForm() {
  emailjs.init(import.meta.env.VITE_EMAILJS_PUBLIC_KEY)

  const serviceID = import.meta.env.VITE_EMAIL_SERVICE_ID
  const templateID = import.meta.env.VITE_EMAILJS_TEMPLATE_ID

  const templateParams = {
    user_name: user.value.lastName,
    from_name: user.value.firstName,
    user_email: user.value.yourMail,
    message: user.value.yourMessages
  }

  emailjs
    .send(serviceID, templateID, templateParams)
    .then(() => {
      alert('Votre message a bien été envoyé.')
      user.value = {
        lastName: '',
        firstName: '',
        yourMail: '',
        yourMessages: ''
      }
    })
    .catch((error) => {
      console.error("Erreur lors de l'envoi du formulaire :", error)
      alert("Une erreur est survenue lors de l'envoi de votre formulaire.")
    })
}
</script>

<style scoped>
.projects-section {
  margin-top: 64px;
}

.projects-title {
  margin-bottom: 56px;
}

.about-content {
  max-width: 1000px;
}

.about-content p {
  margin-bottom: 18px;
}

.contact-section {
  margin-top: 72px;
  width: 100%;
}

.contact-wrapper {
  width: 100%;
  max-width: 900px;
  margin: 0 auto;
}

.contact-title {
  margin-bottom: 14px;
}

.contact-intro {
  margin-bottom: 32px;
  padding-left: 0;
  padding-right: 0;
}

.contact-wrapper form {
  background: #f8f8f8;
  border: 1px solid #e8e8e8;
  border-radius: 10px;
  box-shadow: 0 4px 14px rgba(0, 0, 0, 0.05);
  padding: 28px;
}

.contact-wrapper label {
  display: block;
  margin-top: 12px;
  margin-bottom: 8px;
  font-weight: 600;
}

.contact-wrapper input,
.contact-wrapper textarea {
  width: 100%;
  box-sizing: border-box;
  transition: border-color 0.25s ease, box-shadow 0.25s ease, background-color 0.25s ease;
}

.contact-wrapper input:focus,
.contact-wrapper textarea:focus {
  outline: none;
  border-color: #172432;
  box-shadow: 0 0 0 3px rgba(23, 36, 50, 0.12);
  background-color: white;
}

.contact-wrapper textarea {
  min-height: 180px;
}

.contact-wrapper button[type='submit'] {
  margin-top: 32px;
  align-self: center;
  min-width: 180px;
  font-weight: 600;
}
</style>