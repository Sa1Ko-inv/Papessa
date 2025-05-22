<script setup>
import AboutSection from './section/AboutSection.vue'
import ContactsSection from './section/ContactsSection.vue'
import Header from "./section/Header.vue";
import PriceSection from "./section/PriceSection.vue";
</script>

<template>
  <main class="app">
    <section id="header">
      <Header />
    </section>
    <section id="about">
      <AboutSection />
    </section>
    <section id="price">
      <PriceSection />
    </section>
    <section id="contacts">
      <ContactsSection />
    </section>
  </main>
</template>

<style scoped>
</style>
