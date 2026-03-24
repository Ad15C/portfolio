<template>
  <section class="contact-section">
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
.contact-section {
  width: 100%;
  margin-top: 40px;
}

#myForm {
  width: 100%;
  max-width: 900px;
  margin: 0 auto;
  padding: 28px;
  background: #f8f8f8;
  border: 1px solid #e8e8e8;
  border-radius: 10px;
  box-shadow: 0 4px 14px rgba(0, 0, 0, 0.05);
}

.contact-title {
  margin-bottom: 12px;
}

.contact-intro {
  margin-bottom: 28px;
  padding-left: 0;
  padding-right: 0;
}

#myForm label {
  display: block;
  margin-top: 12px;
  margin-bottom: 8px;
  font-weight: 600;
}

#myForm input,
#myForm textarea {
  width: 100%;
  box-sizing: border-box;
  transition: border-color 0.25s ease, box-shadow 0.25s ease, background-color 0.25s ease;
}

#myForm input:focus,
#myForm textarea:focus {
  outline: none;
  border-color: #172432;
  box-shadow: 0 0 0 3px rgba(23, 36, 50, 0.12);
  background-color: white;
}

#myForm textarea {
  min-height: 180px;
}

#myForm button[type='submit'] {
  margin-top: 32px;
  min-width: 180px;
  font-weight: 600;
}
</style>