<template>
  <section
      class="master-section"
      id="master"
      ref="aboutSection"
      :style="{ paddingTop: sectionPadding, paddingBottom: sectionPadding }"
  >
    <div class="vignette vignette-left"><img src="./../assets/V1L.png" alt=""></div>
    <div class="vignette vignette-right"><img src="./../assets/V1L.png" alt=""></div>
    <div class="container">
      <h2 class="master-title">Мастера, которые у нас работают</h2>

      <div class="master-content">
        <div class="master-card">
          <img src="./../assets/PartretDAHA.jpg" alt="Мастер 1">
          <h3>Дарья Дмитриевна</h3>
          <p>Специализация: реализм, черно-белые татуировки</p>
        </div>
        <div class="master-card">
          <img src="./../assets/partretSOSIYA.jpg" alt="Мастер 2">
          <h3>Сосья Вадимовна</h3>
          <p>Специализация: мини-тату, хоррор и летеринг</p>
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
.master-section {
  position: relative;
  padding: 80px 0;
  background-color: #121212;
  color: #f0f0f0;

  opacity: 0;
  transform: scale(0.95);
  transition: opacity 0.6s ease, transform 0.6s ease;
}

.master-section.visible {
  opacity: 1;
  transform: scale(1);
}

.master-section::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: linear-gradient(90deg, transparent, #adacac, transparent);
}



.master-title {
  font-size: 36px;
  font-weight: 700;
  margin-bottom: 40px;
  color: #ffffff;
  text-align: center;
  position: relative;
}

.master-title::after {
  content: '';
  display: block;
  width: 700px;
  height: 3px;
  background: linear-gradient(90deg, transparent, #adacac, transparent);
  margin: 15px auto 0;
}

.container {
  max-width: 1000px;
  margin: 0 auto;
  background: rgba(20, 20, 20, 0.85);
  padding: 40px;
  border-radius: 16px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.6);
}

.master-content {
  display: flex;
  justify-content: center;
  margin: 0 auto;
  max-width: 1000px;
  gap: 60px;
  padding: 30px 60px;
}

.master-card {
  background-color: #1e1e1e;
  border-radius: 12px;
  overflow: hidden;
  text-align: center;
  color: #ffffff;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
  transition: transform 0.3s ease;
  width: 300px;
  padding: 20px;
}

.master-card:hover {
  transform: scale(1.05);
}

.master-card img {
  width: 100%;
  height: auto;
  border-radius: 8px;
  margin-bottom: 16px;
  object-fit: cover;
}

.master-card h3 {
  font-size: 20px;
  margin-bottom: 10px;
  font-weight: 600;
}

.master-card p {
  font-size: 16px;
  color: #cccccc;
  line-height: 1.6;
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
    width: 320px;
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

@media (min-width: 1921px) {
  .container {
    max-width: 1400px;
    padding: 60px;
  }

  .master-title {
    font-size: 45px;
  }

  .master-title::after {
    width: 800px;
  }

  .master-content {
    gap: 80px;
    padding: 40px 80px;
  }

  .master-card {
    width: 360px;
    padding: 30px;
  }

  .master-card h3 {
    font-size: 24px;
  }

  .master-card p {
    font-size: 18px;
  }

  .vignette {
    width: 700px;
  }
}

@media (max-width: 1024px) {
  .master-section {
    padding: 60px 0;
  }

  .container {
    padding: 30px;
  }

  .master-title {
    font-size: 30px;
  }

  .master-title::after {
    width: 400px;
  }

  .master-content {
    flex-direction: row;
    align-items: center;
    gap: 40px;
    padding: 20px;
  }

  .master-card {
    width: 90%;
    max-width: 340px;
  }

  .master-card h3 {
    font-size: 20px;
  }

  .master-card p {
    font-size: 16px;
  }
}

@media (max-width: 768px) {
  .master-title {
    font-size: 26px;
  }

  .master-title::after {
    width: 300px;
  }

  .master-content {
    gap: 30px;
    padding: 10px;
    flex-direction: row;
  }

  .master-card {
    width: 95%;
    padding: 18px;
  }

  .master-card h3 {
    font-size: 18px;
  }

  .master-card p {
    font-size: 15px;
  }
}

@media (max-width: 480px) {

  .container {
    padding: 20px 0;
  }

  .master-content {
    flex-direction: row;
    gap: 10px;
    padding: 10px;
  }

  .master-title {
    font-size: 22px;
  }

  .master-title::after {
    width: 250px;

  }

  .master-card {
    padding: 15px;
    width: 250px;
    height: 350px;
  }

  .master-card h3 {
    font-size: 17px;
  }

  .master-card p {
    font-size: 14px;
  }
}


</style>