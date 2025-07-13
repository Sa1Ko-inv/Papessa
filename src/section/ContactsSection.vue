<template>
  <section
      class="contact-section"
      id="contact"
      ref="aboutSection"
      :style="{ paddingTop: sectionPadding}"
  >
    <div class="vignette vignette-left"><img src="./../assets/V1L.png" alt=""></div>
    <div class="vignette vignette-right"><img src="./../assets/V1L.png" alt=""></div>

    <div class="container">
      <h2 class="contact-title">Контактная информация</h2>

      <div class="contact-details">
        <p class="contact-item">📍 Адрес: г. Ярославль, ул. Проспект Ленина, д. 14</p>
        <p class="contact-item">📍 Находимся позади дома</p>
        <p class="contact-item">📞 Телефон: +7 (999) 123-12-12</p>
        <p class="contact-item">
          📱 Telegram:
          <a href="https://t.me/tattoo_studio" target="_blank">@tattoo_studio</a>
        </p>
      </div>

      <div class="map-wrapper">
        <iframe src="https://yandex.ru/map-widget/v1/?ll=39.874234%2C57.639977&mode=routes&rtext=~57.640152%2C39.873860&rtt=auto&ruri=~&z=19.4"
                width="100%"
                height="500"
                allowfullscreen
                style="position:relative;"
        ></iframe>
      </div>
    </div>
  </section>
</template>

<script setup>
import {ref, onMounted} from 'vue';

const aboutSection = ref(null);
const sectionPadding = ref('100px');

onMounted(() => {
  const navbarHeight = document.querySelector('.navbar-wrapper')?.offsetHeight || 0;
  sectionPadding.value = `${navbarHeight + 70}px`;

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
.contact-section {
  position: relative;
  background-color: #121212;
  color: #f0f0f0;
  opacity: 0;
  transform: scale(0.95);
  transition: opacity 0.6s ease, transform 0.6s ease;
}

.contact-section.visible {
  opacity: 1;
  transform: scale(1);
}

.contact-section::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: linear-gradient(90deg, transparent, #adacac, transparent);
}

.contact-title {
  font-size: 36px;
  font-weight: 700;
  margin-bottom: 30px;
  color: #ffffff;
  text-align: center;
  position: relative;
}

.contact-title::after {
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

.contact-details {
  text-align: center;
  margin-bottom: 30px;
}

.contact-item {
  font-size: 1.2rem;
  margin: 0.5rem 0;
}

.contact-item a {
  color: #ffcc00;
  text-decoration: none;
}

.contact-item a:hover {
  text-decoration: underline;
}

.map-wrapper {
  width: 100%;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 0 10px rgba(255, 255, 255, 0.1);
}

/* Виньетки */
.vignette {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 500px;
  z-index: 1;
  opacity: 0.6;
  pointer-events: none;
  background-repeat: no-repeat;
  background-size: contain;
  background-position: center;

  img {
    position: relative;
    height: 100%;
    object-fit: cover;
    opacity: 0.6;
  }
}

.vignette-left {
  left: -18vh;
  filter: drop-shadow(0 0 5px silver);
  transform: scaleX(-1);
}

.vignette-right {
  right: -18vh;
  filter: drop-shadow(0 0 5px silver);
}

/* Адаптив */
@media (max-width: 1400px) {

  .vignette-left {
    left: -18vh;
  }

  .vignette-right {
    right: -18vh;
  }
}

@media (max-width: 1150px) {
  .vignette {
    display: none;
  }

  .contact-title::after {
    width: 90%;
  }
}

@media (max-width: 768px) {
  .contact-title {
    font-size: 28px;
  }

  .contact-item {
    font-size: 1rem;
  }

  .container {
    padding: 25px;
  }
}

@media (min-width: 1921px) {
  .container {
    max-width: 1400px;
    padding: 60px;
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
    left: -3.5vw;
    filter: drop-shadow(0 0 5px silver);
    transform: scaleX(-1);
  }

  .vignette-right {
    right: -3.5vw;
    filter: drop-shadow(0 0 5px silver);

  }
}

@media (max-width: 480px) {
  .container {
    padding: 15px;
  }
}
</style>
