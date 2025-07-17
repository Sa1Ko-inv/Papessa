<template>
  <header class="header">
    <div ref="navbarRef" :class="['navbar-wrapper', { sticky: isSticky }]">
      <div class="container">
        <nav class="navbar">
          <div class="navbar-link">
            <a href="#header">
              <img src="../assets/Logo.png" alt="" class="navbar-logo" />
            </a>
          </div>
          <button class="burger" @click="toggleMenu">
            <span :class="{ open: isOpen }"></span>
            <span :class="{ open: isOpen }"></span>
            <span :class="{ open: isOpen }"></span>
          </button>
          <div :class="['navbar-menu', { open: isOpen }]">
            <a @click.prevent="scrollToSection('about')">О нас</a>
            <a @click.prevent="scrollToSection('price')">Цены</a>
            <a @click.prevent="scrollToSection('master')">Мастера</a>
            <a @click.prevent="scrollToSection('work')">Наши работы</a>
            <a @click.prevent="scrollToSection('sketch')">Эскизы</a>
<!--            <a @click.prevent="scrollToSection('merch')">Мерч</a>-->
            <a @click.prevent="scrollToSection('contacts')">Контакты</a>
          </div>
        </nav>
      </div>
    </div>

    <!-- Заполнитель для устранения смещения -->
    <div v-if="isSticky" :style="{ height: navbarHeight + 'px' }"></div>

    <div class="container">
      <div class="header-content">
        <h1>Papessa — тату-студия, где искусство становится частью тебя</h1>
        <p>В Papessa мы верим, что татуировка — это не просто изображение на коже. <br>Это выражение личности, символ силы, памяти или свободы.</p>
        <p>Каждая работа создаётся с вниманием к деталям, уважением к идее и заботой о тебе.</p>
        <p><strong>Индивидуальные эскизы, профессиональные мастера, стерильные условия и тёплая атмосфера —</strong> всё, чтобы ты чувствовал(а) себя уверенно на каждом этапе.</p>
        <p>Будь смелым. Будь собой.</p>
        <h4>Papessa — тату, которое говорит за тебя.</h4>
        <ConsultationForm />
      </div>
    </div>
  </header>
</template>


<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import ConsultationForm from "../components/ConsultationForm.vue";

const isOpen = ref(false)
const isSticky = ref(false)
const navbarRef = ref(null)
const navbarHeight = ref(0)

const toggleMenu = () => {
  isOpen.value = !isOpen.value
}

const handleScroll = () => {
  if (navbarRef.value) {
    const offsetTop = navbarRef.value.offsetTop
    const height = navbarRef.value.offsetHeight
    navbarHeight.value = height
    isSticky.value = window.scrollY > offsetTop + 50
  }
}

// Добавляем функцию для точного скролла
const scrollToSection = (id) => {
  if (isOpen.value) toggleMenu(); // Закрываем бургер-меню если открыто

  const element = document.getElementById(id);
  if (element) {
    const navbarHeight = navbarRef.value?.offsetHeight || 0;
    const offset = element.getBoundingClientRect().top + window.scrollY - navbarHeight;

    window.scrollTo({
      top: offset,
      behavior: 'smooth'
    });
  }
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})
onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>



<style>
:root {
  --max-container: 1400px;
}

/* Базовая стилизация */
.header {
  position: relative;
  background-image: url('../assets/background.png');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  color: white;
  padding-top: 30px;
  padding-bottom: 100px;
  min-height: 100vh;
  overflow: hidden;
}

.header::before {
  content: '';
  position: absolute;
  inset: 0;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 0;
}

.header > * {
  position: relative;
  z-index: 2;
}

.container {
  max-width: var(--max-container);
  padding: 0 40px;
  margin: 0 auto;
}

.navbar-wrapper {
  position: relative;
  transition: all 0.3s ease;
  background-color: transparent;
  z-index: 1000;
}

.navbar-wrapper.sticky {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background-color: rgba(20, 20, 20, 0.98);
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.6);
  z-index: 1001;
}

.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 20px;
  flex-wrap: wrap;
}

.navbar-link img.navbar-logo {
  width: 160px;
  transition: transform 0.3s ease;
}

.navbar-link img.navbar-logo:hover {
  transform: scale(1.05);
}

.navbar-menu {
  display: flex;
  gap: 30px;
  transition: all 0.3s ease;
}

.navbar-menu a {
  font-size: 25px;
  font-weight: 500;
  color: white;
  text-decoration: none;
  transition: all 0.3s;
  padding: 5px 0;
  border-bottom: 2px solid transparent;
  cursor: pointer;
}

.navbar-menu a:hover {
  border-color: #adacac;
  color: #adacac;
}

.burger {
  display: none;
  flex-direction: column;
  justify-content: space-between;
  width: 30px;
  height: 22px;
  background: none;
  border: none;
  cursor: pointer;
  z-index: 1002;
  padding: 0;
}

.burger span {
  display: block;
  height: 3px;
  width: 100%;
  background: white;
  border-radius: 2px;
  transition: all 0.3s ease;
}

.burger span.open:nth-child(1) {
  transform: rotate(45deg) translate(6px, 6px);
}

.burger span.open:nth-child(2) {
  opacity: 0;
}

.burger span.open:nth-child(3) {
  transform: rotate(-45deg) translate(6px, -6px);
}

.header-content {
  max-width: 800px;
  padding: 40px 60px;
  background-color: rgba(0, 0, 0, 0.6);
  border-radius: 16px;
  backdrop-filter: blur(4px);
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.5);
  color: #fff;
  margin-top: 20px;
  animation: fadeInUp 1s ease forwards;
}

.header-content h1 {
  font-size: clamp(24px, 4vw, 55px);
  font-weight: 700;
  margin-bottom: 25px;
  line-height: 1.3;
}

.header-content p {
  font-size: clamp(15px, 2vw, 22px);
  line-height: 1.6;
  margin: 15px 0;
  color: #f0f0f0;
}

.header-content strong {
  color: #ffffff;
  font-weight: 600;
}

.header-content h4 {
  font-size: clamp(18px, 2.2vw, 30px);
  margin: 35px 0;
  font-weight: 600;
  color: #ffffff;
  padding-bottom: 20px;
  border-bottom: 1px solid rgba(240, 240, 240, 0.3);
}

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

/* Большие экраны (≥1920px) */
@media (min-width: 1921px) {
  :root {
    --max-container: 2500px;
  }

  .header {
    background-size: 100% 150vh;
    background-position: center;
    background-repeat: no-repeat;
    background-color: #000; /* чёрный фон под картинкой, если не занимает весь экран */
  }

  .container {
    padding: 0 80px;
  }

  .navbar-menu {
    gap: 40px;
  }

  .navbar-link img.navbar-logo {
    width: 180px;
  }

  .header-content {
    max-width: 1670px;
    padding: 60px 80px;
    margin-top: 100px;
  }

  .header-content h1 {
    font-size: clamp(28px, 4vw, 55px);
    font-weight: 700;
    margin-bottom: 25px;
    line-height: 1.3;
  }

  .header-content p {
    font-size: clamp(23px, 2vw, 28px);
    line-height: 1.6;
    margin: 15px 0;
    color: #f0f0f0;
  }

  .header-content h4 {
    font-size: clamp(18px, 2.2vw, 39px);
  }
}

/* Ноутбуки (1024px–1920px) */
@media (min-width: 1025px) and (max-width: 1920px) {
  .container {
    padding: 0 60px;
  }

  .header {
    background-position: center 30%;
  }

  .header-content {
    max-width: 1670px;
    padding: 60px 80px;
    margin-top: 40px;
  }

}

/* Планшеты (768px–1023px) */
@media (max-width: 1024px) {
  .container {
    padding: 0 30px;
  }

  .navbar-menu {
    display: none;
  }

  .burger {
    display: flex;
  }

  .navbar-menu.open {
    display: flex;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(28, 28, 28, 0.98);
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 25px;
    z-index: 1000;
    opacity: 1;
    pointer-events: auto;
  }

  .navbar-menu.open a {
    font-size: 22px;
    padding: 10px 0;
  }

  .header-content {
    padding: 30px 25px;
    margin-top: 60px;
  }

  .header {
    background-position: center 25%;
    min-height: 85vh;
  }

  .header-content {
    padding: 35px 40px;
  }
}

/* Мобильные (481px–767px) */
@media (max-width: 768px) {
  .container {
    padding: 0 20px;
  }

  .header {
    min-height: 70vh;
    padding-top: 0;
    padding-bottom: 60px;
    background-position: center 20%;
  }

  .navbar-menu {
    display: none;
  }

  .burger {
    display: flex;
  }

  .navbar-menu.open {
    display: flex;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(28, 28, 28, 0.98);
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 25px;
    z-index: 1000;
    opacity: 1;
    pointer-events: auto;
  }

  .navbar-menu.open a {
    font-size: 22px;
    padding: 10px 0;
  }

  .header-content {
    padding: 30px 25px;
    margin-top: 60px;
  }
}

/* Маленькие мобильные (до 480px) */
@media (max-width: 480px) {
  .container {
    padding: 0 15px;
  }

  .navbar-link img.navbar-logo {
    width: 130px;
    position: relative;
    top: 5px;
  }

  .header {
    padding-top: 15px;
    padding-bottom: 40px;
    background-position: center 5%;
    background-size: 600px 100%;
    background-repeat: no-repeat;
    background-color: #000; /* чёрный фон под картинкой, если не занимает весь экран */
    min-height: 70vh;
  }

  .header-content {
    padding: 25px 20px;
    margin-top: 10px;
  }

  .burger {
    width: 25px;
    height: 20px;
  }

  .header-content h4 {
    font-size: clamp(16px, 2vw, 24px);
    margin: 20px 0;
    padding-bottom: 15px;
    border-bottom: 1px solid rgba(240, 240, 240, 0.3);
  }
}
</style>