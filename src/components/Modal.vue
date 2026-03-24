<template>
  <div v-if="isOpenModal && project" class="modal" role="dialog" aria-modal="true">
    <div class="modal-content">
      <div class="modal_header">
        <button type="button" class="close" @click="close" aria-label="Fermer la fenêtre">
          &times;
        </button>
      </div>

      <div class="modal_body">
        <img :src="project.src" :alt="project.title" id="modalProject" />
        <h4 id="projectName">{{ project.title }} - ( {{ project.date }} )</h4>
        <p class="modal-description">{{ project.description }}</p>
        <p class="modal-technologies">
          <span class="underline">Technologies utilisées :</span>&nbsp;
          {{ project.technologies }}
        </p>
      </div>

      <div class="modal_footer">
        <a
          v-if="project.pdfLink"
          :href="project.pdfLink"
          target="_blank"
          rel="noopener noreferrer"
          class="modal-button"
        >
          Voir le PDF
        </a>

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
defineProps({
  isOpenModal: {
    type: Boolean,
    required: true
  },
  project: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['close'])

function close() {
  emit('close')
}
</script>

<style scoped>
.underline {
  text-decoration: underline;
  font-weight: 600;
}

.close {
  font-size: 30px;
  cursor: pointer;
  line-height: 1;
  background: none;
  border: none;
  color: #333;
  padding: 0;
}

.close:hover {
  background: none;
  color: #172432;
  transform: scale(1.1);
}
</style>