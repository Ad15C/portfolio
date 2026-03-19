<template>
  <div id="top">
    <!--Importation des Pages-->
    <MyHeader />

    <Menu />

    <main>
      <RouterView />
    </main>

    <MyFooter />
  </div>
</template>

<script setup>
import { RouterView } from 'vue-router'
import MyHeader from './components/MyHeader.vue'
import Menu from './components/Menu.vue'
import MyFooter from './components/MyFooter.vue'
</script>

<style scoped>
</style>
