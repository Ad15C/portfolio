<template>
  <section class="contact-section">
    <form id="myForm" @submit.prevent="sendForm">
      <h3>Contact</h3>
      <p>Une question ou un projet ? N’hésitez pas à me contacter.</p>

      <label for="lastName">Votre nom :</label>
      <input type="text" id="lastName" v-model.trim="user.lastName" required />

      <label for="firstName">Votre prénom :</label>
      <input type="text" id="firstName" v-model.trim="user.firstName" required />

      <label for="yourMail">Votre email :</label>
      <input type="email" id="yourMail" v-model.trim="user.yourMail" required />

      <label for="yourMessages">Votre message :</label>
      <textarea id="yourMessages" v-model.trim="user.yourMessages" required></textarea>

      <button type="submit">Envoyer</button>
    </form>
  </section>
</template>

<script setup>
import emailjs from '@emailjs/browser'
import { ref } from 'vue'

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
</style>