<template>
  <section
      class="merch-section"
      id="merch"
      ref="aboutSection"
      :style="{ paddingTop: sectionPadding, paddingBottom: sectionPadding }"
  >
    <div class="vignette vignette-left"><img src="./../assets/V1L.png" alt=""></div>
    <div class="vignette vignette-right"><img src="./../assets/V1L.png" alt=""></div>
    <div class="container">
      <h2 class="merch-title">Мерч нашей тату студии</h2>

      <div class="merch-photo">
        <div class="merch-gallery">
          <img src="./../assets/hach1.jpg" alt="hachiko">
          <img src="./../assets/hach1.jpg" alt="hachiko">
          <img src="./../assets/hach1.jpg" alt="hachiko">
          <img src="./../assets/hach1.jpg" alt="hachiko">
          <img src="./../assets/hach1.jpg" alt="hachiko">
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import {ref, onMounted} from 'vue';

const aboutSection = ref(null);
const sectionPadding = ref('100px');

// Для точного позиционирования при скролле
onMounted(() => {
  // Рассчитываем отступ с учетом высоты навбара
  const navbarHeight = document.querySelector('.navbar-wrapper').offsetHeight;
  sectionPadding.value = `${navbarHeight + 70}px`;

  // Наблюдатель для анимации при скролле
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('visible');
      }
    });
  }, {threshold: 0.1});

  if (aboutSection.value) {
    observer.observe(aboutSection.value);
  }
});
</script>

<style scoped>
.merch-section {
  position: relative;
  padding: 80px 0;
  background-color: #121212;
  color: #f0f0f0;

  opacity: 0;
  transform: scale(0.95);
  transition: opacity 0.6s ease, transform 0.6s ease;
}

.merch-section.visible {
  opacity: 1;
  transform: scale(1);
}

.merch-section::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: linear-gradient(90deg, transparent, #adacac, transparent);
}


.merch-photo {
  margin-top: 60px;
  text-align: center;
}

.merch-gallery {
  display: flex;
  justify-content: center;
  gap: 20px;
  flex-wrap: wrap;
}

.merch-gallery img {
  width: 246px;
  height: auto;
  border-radius: 12px;
  object-fit: cover;
  transition: transform 0.3s ease;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
}

.merch-gallery img:hover {
  transform: scale(1.05);
}

.merch-title {
  font-size: 36px;
  font-weight: 700;
  margin-bottom: 40px;
  color: #ffffff;
  text-align: center;
  position: relative;
}

.merch-title::after {
  content: '';
  display: block;
  width: 700px;
  height: 3px;
  background: linear-gradient(90deg, transparent, #adacac, transparent);
  margin: 15px auto 0;
}

.container {
  position: relative;
  max-width: 1000px;
  margin: 0 auto;
  background: rgba(20, 20, 20, 0.85);
  padding: 40px;
  border-radius: 16px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.6);
  z-index: 2;
}

/* Анимация */
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(40px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.vignette {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 500px; /* регулируй по желанию */
  background-repeat: no-repeat;
  background-size: contain;
  background-position: center;
  z-index: 1; /* под контентом, но выше фона */
  opacity: 0.6; /* серебристый эффект */
  pointer-events: none;


  img {
    position: relative;
    height: 100%;
    object-fit: cover;
    opacity: 0.6; /* серебристый эффект */
    transform: scaleY(-1);
  }
}

.vignette-left {
  left: -12vh;
  filter: drop-shadow(0 0 5px silver);
  transform: scaleX(-1);
}

.vignette-right {
  right: -12vh;
  filter: drop-shadow(0 0 5px silver);
}

@media (max-width: 1400px) {
  .vignette {
    width: 420px;
    img {
      position: relative;
      height: 100%;
      object-fit: cover;
      opacity: 0.6; /* серебристый эффект */
    }
  }
  .vignette-left {
    left: -9vh;
    filter: drop-shadow(0 0 5px silver);
    transform: scaleX(-1);
  }

  .vignette-right {
    right: -9vh;
    filter: drop-shadow(0 0 5px silver);

  }
}

/* Мобильная адаптация */
@media (max-width: 1150px) {
  .vignette {
    display: none;
  }
}

/* ======= >1920px (Большие экраны) ======= */
@media (min-width: 1921px) {
  .container {
    max-width: 1400px;
    padding: 60px;
  }

  .merch-title {
    font-size: 48px;
  }

  .merch-title::after {
    width: 900px;
  }

  .merch-gallery img {
    width: 400px;
  }

  .vignette {
    img {
      max-width: 590px;
      position: relative;
      height: 100%;
      object-fit: cover;
      opacity: 0.6; /* серебристый эффект */
    }
  }
  .vignette-left {
    left: 0;
    filter: drop-shadow(0 0 5px silver);
    transform: scaleX(-1);
  }

  .vignette-right {
    right: 0;
    filter: drop-shadow(0 0 5px silver);

  }
}

/* ======= max-width: 1024px (Планшеты) ======= */
@media (max-width: 1024px) {
  .container {
    padding: 30px;
  }

  .merch-title {
    font-size: 32px;
  }

  .merch-title::after {
    width: 500px;
  }

  .merch-gallery img {
    width: 290px;
  }
}

/* ======= max-width: 768px (Мобильные) ======= */
@media (max-width: 768px) {
  .merch-title {
    font-size: 26px;
  }

  .merch-title::after {
    width: 300px;
  }

  .merch-photo {
    margin-top: 40px;
  }

  .container {
    padding: 20px;
  }

  .merch-gallery img {
    width: 220px;
  }
}

/* ======= max-width: 480px (Маленькие экраны) ======= */
@media (max-width: 480px) {
  .merch-title {
    font-size: 22px;
    margin-bottom: 20px;
  }

  .merch-title::after {
    width: 250px;
  }

  .merch-photo {
    margin-top: 40px;
  }

  .container {
    padding: 16px;
  }

  .merch-gallery img {
    width: 150px;
  }
}

</style>





<!--<template>-->
<!--  <section-->
<!--      class="merch-section"-->
<!--      id="merch"-->
<!--      ref="aboutSection"-->
<!--      :style="{ paddingTop: sectionPadding, paddingBottom: sectionPadding }"-->
<!--  >-->
<!--    <div class="vignette vignette-left"><img src="./../assets/V1L.png" alt=""></div>-->
<!--    <div class="vignette vignette-right"><img src="./../assets/V1L.png" alt=""></div>-->
<!--    <div class="container">-->
<!--      <h2 class="merch-title">Мерч нашей тату студии</h2>-->

<!--      <div class="merch-category">-->
<!--        <h3 class="merch-subtitle">Футболки</h3>-->
<!--        <div class="merch-gallery">-->
<!--          <img src="./../assets/hach1.jpg" alt="Футболка 1">-->
<!--          <img src="./../assets/hach1.jpg" alt="Футболка 2">-->
<!--          <img src="./../assets/hach1.jpg" alt="Футболка 3">-->
<!--        </div>-->
<!--      </div>-->

<!--      <div class="merch-category">-->
<!--        <h3 class="merch-subtitle">Худи</h3>-->
<!--        <div class="merch-gallery">-->
<!--          <img src="./../assets/hach1.jpg" alt="Худи 1">-->
<!--          <img src="./../assets/hach1.jpg" alt="Худи 2">-->
<!--          <img src="./../assets/hach1.jpg" alt="Худи 3">-->
<!--        </div>-->
<!--      </div>-->
<!--    </div>-->
<!--  </section>-->
<!--</template>-->


<!--<script setup>-->
<!--import {ref, onMounted} from 'vue';-->

<!--const aboutSection = ref(null);-->
<!--const sectionPadding = ref('100px');-->

<!--// Для точного позиционирования при скролле-->
<!--onMounted(() => {-->
<!--  // Рассчитываем отступ с учетом высоты навбара-->
<!--  const navbarHeight = document.querySelector('.navbar-wrapper').offsetHeight;-->
<!--  sectionPadding.value = `${navbarHeight + 70}px`;-->

<!--  // Наблюдатель для анимации при скролле-->
<!--  const observer = new IntersectionObserver((entries) => {-->
<!--    entries.forEach(entry => {-->
<!--      if (entry.isIntersecting) {-->
<!--        entry.target.classList.add('visible');-->
<!--      }-->
<!--    });-->
<!--  }, {threshold: 0.1});-->

<!--  if (aboutSection.value) {-->
<!--    observer.observe(aboutSection.value);-->
<!--  }-->
<!--});-->
<!--</script>-->

<!--<style scoped>-->
<!--.merch-section {-->
<!--  position: relative;-->
<!--  padding: 80px 0;-->
<!--  background-color: #121212;-->
<!--  color: #f0f0f0;-->

<!--  opacity: 0;-->
<!--  transform: scale(0.95);-->
<!--  transition: opacity 0.6s ease, transform 0.6s ease;-->
<!--}-->

<!--.merch-section.visible {-->
<!--  opacity: 1;-->
<!--  transform: scale(1);-->
<!--}-->

<!--.merch-section::before {-->
<!--  content: '';-->
<!--  position: absolute;-->
<!--  top: 0;-->
<!--  left: 0;-->
<!--  right: 0;-->
<!--  height: 2px;-->
<!--  background: linear-gradient(90deg, transparent, #adacac, transparent);-->
<!--}-->

<!--.merch-category {-->
<!--  margin-top: 60px;-->
<!--}-->

<!--.merch-subtitle {-->
<!--  font-size: 24px;-->
<!--  font-weight: 600;-->
<!--  margin-bottom: 20px;-->
<!--  text-align: center;-->
<!--  color: #dddddd;-->
<!--  position: relative;-->
<!--}-->

<!--.merch-subtitle::after {-->
<!--  content: '';-->
<!--  display: block;-->
<!--  width: 150px;-->
<!--  height: 2px;-->
<!--  background: linear-gradient(90deg, transparent, #888, transparent);-->
<!--  margin: 10px auto 0;-->
<!--}-->

<!--.merch-gallery {-->
<!--  display: flex;-->
<!--  justify-content: center;-->
<!--  gap: 20px;-->
<!--  flex-wrap: wrap;-->
<!--  animation: fadeInUp 0.8s ease-in-out;-->
<!--}-->

<!--.merch-gallery img {-->
<!--  width: 230px;-->
<!--  height: auto;-->
<!--  border-radius: 14px;-->
<!--  object-fit: cover;-->
<!--  transition: transform 0.3s ease, box-shadow 0.3s ease;-->
<!--  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.5);-->
<!--}-->

<!--.merch-gallery img:hover {-->
<!--  transform: scale(1.06);-->
<!--  box-shadow: 0 10px 30px rgba(255, 255, 255, 0.15);-->
<!--}-->


<!--.merch-gallery {-->
<!--  display: flex;-->
<!--  justify-content: center;-->
<!--  gap: 20px;-->
<!--  flex-wrap: wrap;-->
<!--}-->

<!--.merch-gallery img {-->
<!--  width: 246px;-->
<!--  height: auto;-->
<!--  border-radius: 12px;-->
<!--  object-fit: cover;-->
<!--  transition: transform 0.3s ease;-->
<!--  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);-->
<!--}-->

<!--.merch-gallery img:hover {-->
<!--  transform: scale(1.05);-->
<!--}-->

<!--.merch-title {-->
<!--  font-size: 36px;-->
<!--  font-weight: 700;-->
<!--  margin-bottom: 40px;-->
<!--  color: #ffffff;-->
<!--  text-align: center;-->
<!--  position: relative;-->
<!--}-->

<!--.merch-title::after {-->
<!--  content: '';-->
<!--  display: block;-->
<!--  width: 700px;-->
<!--  height: 3px;-->
<!--  background: linear-gradient(90deg, transparent, #adacac, transparent);-->
<!--  margin: 15px auto 0;-->
<!--}-->

<!--.container {-->
<!--  position: relative;-->
<!--  max-width: 1000px;-->
<!--  margin: 0 auto;-->
<!--  background: rgba(20, 20, 20, 0.85);-->
<!--  padding: 40px;-->
<!--  border-radius: 16px;-->
<!--  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.6);-->
<!--  z-index: 2;-->
<!--}-->

<!--/* Анимация */-->
<!--@keyframes fadeInUp {-->
<!--  from {-->
<!--    opacity: 0;-->
<!--    transform: translateY(40px);-->
<!--  }-->
<!--  to {-->
<!--    opacity: 1;-->
<!--    transform: translateY(0);-->
<!--  }-->
<!--}-->


<!--.vignette {-->
<!--  position: absolute;-->
<!--  top: 0;-->
<!--  bottom: 0;-->
<!--  width: 500px; /* регулируй по желанию */-->
<!--  background-repeat: no-repeat;-->
<!--  background-size: contain;-->
<!--  background-position: center;-->
<!--  z-index: 1; /* под контентом, но выше фона */-->
<!--  opacity: 0.6; /* серебристый эффект */-->
<!--  pointer-events: none;-->


<!--  img {-->
<!--    position: relative;-->
<!--    height: 100%;-->
<!--    object-fit: cover;-->
<!--    opacity: 0.6; /* серебристый эффект */-->
<!--    transform: scaleY(-1);-->
<!--  }-->
<!--}-->

<!--.vignette-left {-->
<!--  left: -12vh;-->
<!--  filter: drop-shadow(0 0 5px silver);-->
<!--  transform: scaleX(-1);-->
<!--}-->

<!--.vignette-right {-->
<!--  right: -12vh;-->
<!--  filter: drop-shadow(0 0 5px silver);-->
<!--}-->

<!--@media (max-width: 1400px) {-->
<!--  .vignette {-->
<!--    width: 420px;-->

<!--    img {-->
<!--      position: relative;-->
<!--      height: 100%;-->
<!--      object-fit: cover;-->
<!--      opacity: 0.6; /* серебристый эффект */-->
<!--    }-->
<!--  }-->

<!--  .vignette-left {-->
<!--    left: -9vh;-->
<!--    filter: drop-shadow(0 0 5px silver);-->
<!--    transform: scaleX(-1);-->
<!--  }-->

<!--  .vignette-right {-->
<!--    right: -9vh;-->
<!--    filter: drop-shadow(0 0 5px silver);-->

<!--  }-->
<!--}-->

<!--/* Мобильная адаптация */-->
<!--@media (max-width: 1150px) {-->
<!--  .vignette {-->
<!--    display: none;-->
<!--  }-->
<!--}-->

<!--/* ======= >1920px (Большие экраны) ======= */-->
<!--@media (min-width: 1921px) {-->
<!--  .container {-->
<!--    max-width: 1400px;-->
<!--    padding: 60px;-->
<!--  }-->

<!--  .merch-title {-->
<!--    font-size: 48px;-->
<!--  }-->

<!--  .merch-title::after {-->
<!--    width: 900px;-->
<!--  }-->

<!--  .merch-gallery img {-->
<!--    width: 320px;-->
<!--  }-->
<!--.vignette {-->
<!--img {-->
<!--max-width: 590px;-->
<!--position: relative;-->
<!--height: 100%;-->
<!--object-fit: cover;-->
<!--opacity: 0.6; /* серебристый эффект */-->
<!--}-->
<!--}-->
<!--.vignette-left {-->
<!--left: 0;-->
<!--filter: drop-shadow(0 0 5px silver);-->
<!--transform: scaleX(-1);-->
<!--}-->

<!--.vignette-right {-->
<!--right: 0;-->
<!--filter: drop-shadow(0 0 5px silver);-->

<!--}-->
<!--}-->

<!--/* ======= max-width: 1024px (Планшеты) ======= */-->
<!--@media (max-width: 1024px) {-->
<!--  .container {-->
<!--    padding: 30px;-->
<!--    margin-bottom: 30px;-->
<!--  }-->

<!--  .merch-title {-->
<!--    font-size: 32px;-->
<!--  }-->

<!--  .merch-title::after {-->
<!--    width: 500px;-->
<!--  }-->

<!--  .merch-category {-->
<!--    margin-top: 40px;-->
<!--  }-->

<!--  .container {-->
<!--    padding: 40px 20px;-->
<!--  }-->

<!--  .merch-gallery img {-->
<!--    width: 200px;-->
<!--  }-->
<!--}-->

<!--/* ======= max-width: 768px (Мобильные) ======= */-->
<!--@media (max-width: 768px) {-->
<!--  .merch-title {-->
<!--    font-size: 26px;-->
<!--    margin-bottom: 20px;-->
<!--  }-->

<!--  .merch-title::after {-->
<!--    width: 300px;-->
<!--  }-->

<!--  .merch-category {-->
<!--    margin-top: 40px;-->
<!--  }-->

<!--  .container {-->
<!--    padding: 40px 20px;-->
<!--  }-->

<!--  .merch-gallery img {-->
<!--    width: 170px;-->
<!--  }-->
<!--}-->

<!--/* ======= max-width: 480px (Маленькие экраны) ======= */-->
<!--@media (max-width: 480px) {-->
<!--  .merch-title {-->
<!--    font-size: 22px;-->
<!--    margin-bottom: 20px;-->
<!--  }-->

<!--  .merch-title::after {-->
<!--    width: 250px;-->
<!--  }-->

<!--  .container {-->
<!--    padding: 30px 16px;-->
<!--  }-->

<!--  .merch-category {-->
<!--    margin-top: 40px;-->
<!--  }-->

<!--  .merch-gallery img {-->
<!--    width: 140px;-->
<!--  }-->
<!--}-->
<!--</style>-->