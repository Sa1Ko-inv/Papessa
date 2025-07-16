<template>
  <section
      class="contact-section"
      id="contact"
      ref="aboutSection"
      :style="{ paddingTop: sectionPadding }"
  >
    <div class="vignette vignette-left"><img src="./../assets/V1L.png" alt=""></div>
    <div class="vignette vignette-right"><img src="./../assets/V1L.png" alt=""></div>

    <div class="container">
      <h2 class="contact-title">Контактная информация</h2>

      <div class="contact-details">
        <ul class="contact-list">
          <li>
            <img src="./../assets/location.svg" class="icon" alt="location" />
            Адрес: г. Ярославль, ул. Проспект Ленина, д. 14
          </li>
          <li>
            <img src="./../assets/location.svg" class="icon" alt="location" />
            Находимся позади дома
          </li>
          <li @click="copyPhone" class="phone-clickable">
            <img src="./../assets/phone.svg" class="icon" alt="phone" />
            Телефон: +7 (908) 037-22-57
            <span v-if="copied" class="copied-hint">Скопировано!</span>
          </li>
        </ul>

        <div class="social-links">
          <a href="https://t.me/papessa_2" target="_blank" rel="noopener noreferrer">
            <img src="./../assets/telegram.svg" class="icon" alt="telegram" />
            Telegram Дарьи
          </a>
          <a href="https://m.vk.com/papessa_tattoo" target="_blank" rel="noopener noreferrer">
            <img src="./../assets/vk.svg" class="icon" alt="vk" />
            Вк Группа
          </a>
          <a href="https://t.me/urkmsv" target="_blank" rel="noopener noreferrer">
            <img src="./../assets/telegram.svg" class="icon" alt="telegram" />
            Telegram Софьи
          </a>
        </div>
      </div>

      <div class="map-wrapper">
        <iframe
            src="https://yandex.ru/map-widget/v1/?ll=39.874234%2C57.639977&mode=routes&rtext=~57.640152%2C39.873860&rtt=auto&ruri=~&z=19.4"
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
const copied = ref(false);

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

function copyPhone() {
  const phone = '+7 (908) 037-22-57';
  navigator.clipboard.writeText(phone).then(() => {
    copied.value = true;
    setTimeout(() => copied.value = false, 2000);
  });
}

const icons = {
  phone: `<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" viewBox="0 0 24 24"><path d="M17.707 12.293l-1.414 1.414a1 1 0 01-1.414 0l-1.586-1.586a1 1 0 010-1.414l1.586-1.586a1 1 0 011.414 0l1.414 1.414a1 1 0 010 1.414z"/><path d="M21 3H3v18h18V3zM5 19V5h14v14H5z"/></svg>`,
  location: `<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" viewBox="0 0 24 24"><path d="M12 2C8.13 2 5 5.13 5 9c0 5.25 7 13 7 13s7-7.75 7-13c0-3.87-3.13-7-7-7zm0 9.5a2.5 2.5 0 110-5 2.5 2.5 0 010 5z"/></svg>`,
  telegram: `<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" viewBox="0 0 24 24"><path d="M21.05 2.76L2.36 10.1c-.9.34-.9.9-.15 1.12l4.77 1.49 2.2 6.92c.3.9.52 1.12 1.05 1.12s.78-.37 1.08-.67l2.59-2.51 5.4 3.98c.98.54 1.68.26 1.92-.9l3.5-16.5c.3-1.35-.5-1.95-1.87-1.07z"/></svg>`,
  vk: `<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" viewBox="0 0 24 24"><path d="M5.4 3h13.2C21.06 3 22 3.94 22 5.4v13.2c0 1.46-.94 2.4-2.4 2.4H5.4C3.94 21 3 20.06 3 18.6V5.4C3 3.94 3.94 3 5.4 3zm1.65 5.85c-.03.06-.06.13-.08.2-.43 1.26-.63 2.73.45 3.73.65.6 1.48.95 2.34 1.23.56.17.9.27.7.89-.38 1.2-.7 2.33-.72 3.56-.01.47.12.76.69.65 1.94-.36 3.92-.4 5.9-.4 1.3 0 2.08-.47 2.08-1.86V9.62c0-1.1-.83-1.73-1.94-1.73H7.08c-.04 0-.08.02-.12.05z"/></svg>`
};
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
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.2rem;
  margin-bottom: 30px;
}

.contact-list {
  list-style: none;
  padding: 0;
  margin: 0;
  font-size: 1.2rem;
}

.contact-list li {
  margin: 0.4rem 0;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.social-links {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 1.2rem;
  margin-top: 1rem;
}

.social-links a {
  color: #ffcc00;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.3s ease;
}

.social-links a:hover {
  color: #ffe066;
  text-decoration: underline;
}

.icon {
  width: 20px;
  height: 20px;
  margin-right: 8px;
  vertical-align: middle;
}


.phone-clickable {
  cursor: pointer;
  position: relative;
  display: inline-block;
  transition: color 0.3s ease;
}

.phone-clickable:hover {
  color: #ffe066;
}

.copied-hint {
  font-size: 0.85rem;
  color: #8fff8f;
  margin-left: 10px;
  animation: fadeOut 2s forwards;
}

@keyframes fadeOut {
  0% { opacity: 1; }
  80% { opacity: 1; }
  100% { opacity: 0; }
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

  .social-links {
    flex-direction: column;
    align-items: center;
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
