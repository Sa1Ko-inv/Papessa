<script setup>
import AboutPage from './section/AboutPage.vue'
import ContactsPage from './section/ContactsPage.vue'
import Header from "./section/Header.vue";
</script>

<template>
  <main class="app">
    <section id="header">
      <Header />
    </section>
    <section id="about">
      <AboutPage />
    </section>
    <section id="contacts">
      <ContactsPage />
    </section>
  </main>
</template>

<style scoped>
</style>
