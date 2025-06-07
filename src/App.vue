<script setup>
import AboutSection from './section/AboutSection.vue'
import ContactsSection from './section/ContactsSection.vue'
import Header from "./section/Header.vue";
import PriceSection from "./section/PriceSection.vue";
import MasterSection from "./section/MasterSection.vue";
import WorkSection from "./section/WorkSection.vue";
import SketchSection from "./section/SketchSection.vue";
import MerchSection from "./section/MerchSection.vue";
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
    <section id="master">
      <MasterSection />
    </section>
    <section id="work">
      <WorkSection />
    </section>
    <section id="sketch">
      <SketchSection />
    </section>
    <section id="merch">
      <MerchSection />
    </section>
    <section id="contacts">
      <ContactsSection />
    </section>
  </main>
</template>

<style scoped>
</style>
