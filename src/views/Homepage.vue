<template>
  <!--Section A Propos-->
  <section id="A_propos">
    <div>
    <p>
      Je m'appelle Adeline Canon et j'aime tout ce qui se rapporte à la technologie, et suis également passsionnée de sport.
      Mon but est de vous permettre de réaliser tous vos projets, que ce soit un site web, une application mobile.<br>
      Je suis prête à relever les défis qui s'offrent à moi.
    </p>
    <br />
    </div>
  </section>
  <!--Fin Section A Propos-->

  <!--Section Projets-->
  <section id="Projets">
    <h3>Ci-dessous, vous trouverez les projets réalisés dernièrement:</h3>
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
        :is-open-modal="isModalOpen"
        :project="selectedProject"
        @close="closeModal"
      />
    </div>
  </section>
  <!--Fin Section Projets-->

  <!--Formulaire de Contact-->
  <div>
  <form id="myForm" @submit.prevent="sendForm">
    <h3>Pour toutes informations, remplissez le formulaire ci-dessous.</h3>

    <br /><br />
    <!--Champs du Formulaire-->
    <label for="lastName">Votre Nom:</label>
    <input type="text" id="lastName" v-model.trim="user.lastName" required /><br />

    <label for="firstName">Votre Prénom:</label>
    <input type="text" id="firstName" v-model.trim="user.firstName" required /><br />

    <label for="yourMail">Votre Email:</label>
    <input type="email" id="yourMail" v-model.trim="user.yourMail" required />
    <br />

    <label for="yourMessages">Votre Message:</label>
    <textarea id="yourMessages" v-model.trim="user.yourMessages" required /><br />

    <!--Bouton de Soumission-->
    <button type="submit" value="submit">Envoyer</button>
  </form>
  </div>
  <!--Fin du Formulaire-->
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import Modal from '@/components/Modal.vue'
import emailjs from '@emailjs/browser'


//Détails pour les différents projets
const projects = ref([
  {
    id: 1,
    src: '/Images_projets/cv.png',
    title: 'CV',
    date: 'Décembre 2023',
    description: 'Réalisation Curriculum Vitae',
    technologies: 'HTML, CSS, GitHub, VSCode',
    githubLink: 'https://github.com/Ad15C/Mon_cv.git'
  },

  {
    id: 2,
    src: '/Images_projets/cahier_des_charges.png',
    title: 'Cahier des Charges',
    date: 'Décembre 2023',
    description:'Rédaction Cahier des Charges, pour La Socketterie, souhaitant effectuer une refonte de son site internet et un bilan de la concurrence',
    technologies: 'Word',
    pdfLink: '/PDF/Cahier_des_charges.pdf'
  },

  {
    id: 3,
    src: '/Images_projets/commentaire_dynamique.png',
    title: 'Commentaire Dynamique',
    date: 'Février 2024',
    description: 'Réalisation de Commentaire Dynamique ',
    technologies: 'HTML, CSS, GitHub, VSCode, JavaScript',
    githubLink: 'https://github.com/Ad15C/mon_commentaire.git'
  },

  {
    id: 4,
    src: '/Images_projets/prototype_desktop_probeats.png',
    title: 'Maquetter le site Probeats avec Figma',
    date: 'Juillet 2024',
    description: 'Réalisation de Maquettage ',
    technologies: 'Figma',
    githubLink: 'https://github.com/Ad15C/Figma.git'
  },

  {
    id: 5,
    src: '/Images_projets/cv_react_homepage.jpg',
    title: 'CV en ligne avec React.js',
    date: 'Août 2024',
    description: 'Réalisation d\'un CV en ligne avec React.js',
    technologies: 'React.js, HTML, CSS, Sass, Bootstrap, JavaScript, Vite',
    githubLink: 'https://github.com/Ad15C/mon_cv_react.git'
  },

  {
    id: 6,
    src: '/Images_projets/site_wordpress_homepage.png',
    title: 'Site Wordpress',
    date: 'Août 2024',
    description: 'Réalisation d\'un site web avec Wordpress pour l\'entreprise La Vie des Plantes',
    technologies: 'Wordpress, HTML, CSS, JavaScript',
    githubLink: 'https://github.com/Ad15C/La-Vie-Des-Plantes.git'
  },

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
    id: 9,
    src: '/Images_projets/Tifosi.png',
    title: 'Conception d\'une base de données',
    date: 'Janvier 2025',
    description: 'Réalisation d\'une base de données pour le site Tifosi avec MySQL',
    technologies: 'MySQL, MySQL Workbench',
    githubLink: 'https://github.com/Ad15C/tifosi.git'
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
const selectedProject = ref(null)

const isModalOpen = ref(false)

//Ouverture du Modal
const openModal = (project) => {
  selectedProject.value = project
  isModalOpen.value = true
}

//Fermeture du Modal
const closeModal = () => {
  selectedProject.value = null
  isModalOpen.value = false
}

//Fermeture en cliquant en dehors du modal
const clickOutside = (event) => {
  if (isModalOpen.value && event.target.classList.contains('modal')) {
    closeModal()
  }
}

//Fermeture en appuyant sur Escape
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

//Section Formulaire de Contact
const user = ref({
  lastName: '',
  firstName: '',
  yourMail: '',
  yourMessages: ''
})

//Envoi du Formulaire
function sendForm() {
  //Initialisation EMAILJS
  emailjs.init(import.meta.env.VITE_EMAILJS_PUBLIC_KEY)

  //Données d'EmailJs
  const serviceID = import.meta.env.VITE_EMAIL_SERVICE_ID
  const templateID = import.meta.env.VITE_EMAILJS_TEMPLATE_ID

  //Construction du test pour Emailjs
  const test = {
    user_name: user.value.lastName,
    from_name: user.value.firstName,
    user_email: user.value.yourMail,
    message: user.value.yourMessages
  }

  //Soumission du formulaire
  emailjs
    .send(serviceID, templateID, test)
    .then(() => {
      alert('Votre message a bien été envoyé')
      user.value = {
        lastName: '',
        firstName: '',
        yourMail: '',
        yourMessages: ''
      }
    })

    .catch((error) => {
      console.log("Erreur lors de l'envoi du formulaire", error)
      alert("Une erreur est survenue lors de l'envoi de votre formulaire")
    })
}
</script>

<style scoped>
</style>
