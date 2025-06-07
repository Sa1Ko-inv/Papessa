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
            <a href="#price">Цены</a>
            <a href="#master">Мастера</a>
            <a href="#work">Наши работы</a>
            <a href="#sketch">Эскизы</a>
            <a href="#merch">Мерч</a>
            <a href="#contacts">Контакты</a>
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

/* navbar обёртка */
.navbar-wrapper {
  position: relative;
  transition: all 0.3s ease;
  background-color: transparent;
  z-index: 1000; /* Базовый z-index для навбара */
}

.navbar-wrapper.sticky {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background-color: rgba(20, 20, 20, 0.98);
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.6);
  z-index: 1001; /* Выше базового z-index и фона */
}

.container {
  max-width: 1400px;
  padding: 0 40px;
  margin: 0 auto;
}

.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 20px;
}

.navbar-link img.navbar-logo {
  width: 160px;
  height: auto;
}

.navbar-menu {
  display: flex;
  gap: 30px;
  transition: all 0.3s ease;
}

.navbar-menu a {
  font-size: 18px;
  font-weight: 500;
  color: white;
  text-decoration: none;
  transition: color 0.3s;
  cursor: pointer;
}
.navbar-menu a:hover {
  border-color: #adacac;
  color: #adacac;
}

/* Бургер */
.burger {
  display: none;
  flex-direction: column;
  justify-content: space-between;
  width: 25px;
  height: 20px;
  background: none;
  border: none;
  cursor: pointer;
  z-index: 1001;
}

.burger span {
  display: block;
  height: 3px;
  width: 100%;
  background: white;
  border-radius: 2px;
  transition: all 0.3s;
}
.burger span.open:nth-child(1) {
  transform: rotate(45deg) translate(5px, 5px);
}
.burger span.open:nth-child(2) {
  opacity: 0;
}
.burger span.open:nth-child(3) {
  transform: rotate(-45deg) translate(5px, -5px);
}

/* Mobile */
@media (max-width: 883px) {
  .burger {
    display: flex;
    position: relative;
    z-index: 1002; /* Важно: выше чем у меню */
  }

  .navbar-menu {
    position: fixed; /* Изменено с absolute на fixed */
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(28, 28, 28, 0.98);
    flex-direction: column;
    align-items: center;
    justify-content: center;
    max-height: 100vh;
    height: 0;
    overflow: hidden;
    opacity: 0;
    pointer-events: none;
    z-index: 1001; /* Выше основного контента */
    transition: all 0.4s ease;
    padding: 0;
    margin-top: 0;
  }

  .navbar-menu.open {
    height: 100vh;
    opacity: 1;
    pointer-events: auto;
    padding: 20px 0;
  }

  .navbar-menu a {
    padding: 15px 0;
    font-size: 22px;
    width: 100%;
    text-align: center;
  }
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
  font-size: 45px;
  font-weight: 700;
  margin-bottom: 25px;
  line-height: 1.3;
}

.header-content p {
  font-size: 20px;
  line-height: 1.6;
  margin: 15px 0;
  color: #f0f0f0;
}

.header-content strong {
  color: #ffffff;
  font-weight: 600;
}

.header-content h4 {
  font-size: 26px;
  margin: 35px 0 35px 0;
  font-weight: 600;
  color: #ffffff;
  padding-bottom: 20px;
  border-bottom: 1px solid #f0f0f0;
}



/* Анимация появления */
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

/* Адаптив */
@media (max-width: 768px) {
  .header-content {
    padding: 30px 20px;
    margin-top: 60px;
  }

  .header-content h1 {
    font-size: 34px;
  }

  .header-content p {
    font-size: 16px;
  }

  .header-content h4 {
    font-size: 20px;
  }

  .hero__button {
    width: 100%;
    text-align: center;
  }
}

</style>



