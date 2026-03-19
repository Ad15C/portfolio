<template>
  <!--Modal-->
  <div v-if="isOpenModal" class="modal">
    <div class="modal-content">
      <div class="modal_header">
        <!--Croix pour fermer le modal-->
        <span class="close" @click="close">&times;</span>
      </div>

      <!--Détail du modal-->
      <div class="modal_body">
        <img :src="project.src" :alt="project.title" id="modalProject" />
        <h4 id="projectName">{{ project.title }} - ( {{ project.date }} )</h4>
        <p class="modal-description">{{ project.description }}</p>
        <p class="modal-technologies">Technologies utilisées : {{ project.technologies }}</p>
      </div>

      <div class="modal_footer">
        <!--Bouton pour télécharger le PDF duprojet-->
        <a
          v-if="project.pdfLink"
          :href="project.pdfLink"
          target="_blank"
          rel="noopener noreferrer"
          class="modal-button"
        >
          Voir le PDF
        </a>

        <!--Bouton pour télécharger le projet sur GitHub-->
        <a
          v-if="project.githubLink"
          :href="project.githubLink"
          target="_blank"
          rel="noopener noreferrer"
          class="modal-button"
        >
          Voir GitHub
        </a>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue'

const props = defineProps({
  isOpenModal: {
    type: Boolean,
    required: true
  },

  project: {
    type: Object,
    required: true
  }
})

//Emit permet à un composant de spécifier quels événements il émet ainsi que les arguments qu'il prend
const emit = defineEmits(['close'])

//Fermeture du Modal
function close() {
  emit('close')
}

</script>

<style scoped>
</style>
