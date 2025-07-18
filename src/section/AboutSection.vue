<template>
  <section
      id="about"
      class="about"
      ref="aboutSection"
      :style="{ paddingTop: sectionPadding, paddingBottom: sectionPadding }"
  >
    <div class="vignette vignette-left"><img src="./../assets/V1L.png" alt=""></div>
    <div class="vignette vignette-right"><img src="./../assets/V1L.png" alt=""></div>
    <div class="container">
      <div class="about-content">
        <h2 class="about-title">О нас</h2>
        <div class="about-text">
          <p>Тату-студия <strong>Papessa</strong> — это место, где искусство встречается с индивидуальностью. Мы создаём уникальные татуировки, отражающие ваш стиль, характер и историю.</p>

          <p>Наши мастера — профессионалы с многолетним опытом, которые работают в разных стилях: от традиционных и олдскульных до реализма, минимализма и абстракции.</p>

          <p>В <strong>Papessa</strong> мы ценим безопасность, креативность и внимание к деталям. Каждый эскиз разрабатывается индивидуально, а стерильность и комфорт клиента — наш приоритет.</p>

          <p class="highlight">Хотите татуировку, которая будет радовать вас годами? Доверьтесь нам, и мы воплотим ваши идеи в жизнь!</p>
        </div>

        <div class="about-photo">
          <h2 class="studio-title">Студия, в которой мы вас ждём</h2>
          <div class="studio-gallery">
            <img src="./../assets/dver.jpg" alt="dver">
            <img src="./../assets/osnovaya-stud.jpg" alt="osnovaya">
            <img src="./../assets/osnovaya-stud-1.jpg" alt="osnovaya-1">
            <img src="./../assets/dop-stud.jpg" alt="dop">
            <img src="./../assets/dop-stud-1.jpg" alt="dop-1">
          </div>
        </div>

      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const aboutSection = ref(null);
const sectionPadding = ref('100px');

// Для точного позиционирования при скролле
onMounted(() => {
  // Рассчитываем отступ с учетом высоты навбара
  const navbarHeight = document.querySelector('.navbar-wrapper').offsetHeight;
  sectionPadding.value = `${navbarHeight + 30}px`;

  // Наблюдатель для анимации при скролле
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('visible');
      }
    });
  }, { threshold: 0.1 });

  if (aboutSection.value) {
    observer.observe(aboutSection.value);
  }
});
</script>

<style scoped>
.about {
  position: relative;
  padding: 100px 0;
  background-color: #121212;
  color: #f0f0f0;
  overflow: hidden;

  opacity: 0;
  transform: scale(0.95);
  transition: opacity 0.6s ease, transform 0.6s ease;
}

.about.visible {
  opacity: 1;
  transform: scale(1);
}

.about::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: linear-gradient(90deg, transparent, #adacac, transparent);
}

.vignette {
  position: absolute;
  top: 0;
  bottom: 0;
  background-repeat: no-repeat;
  background-size: contain;
  background-position: center;
  z-index: 1; /* под контентом, но выше фона */
  opacity: 0.6; /* серебристый эффект */
  pointer-events: none;


  img {
    position: relative;
    width: 30vw;
    height: 100%;
    object-fit: cover;
    opacity: 0.6; /* серебристый эффект */
  }
}

.vignette-left {
  left: -5vw;
  filter: drop-shadow(0 0 5px silver);
  transform: scaleX(-1);
}

.vignette-right {
  right: -5vw;
  filter: drop-shadow(0 0 5px silver);

}
@media (max-width: 1670px) {
  .vignette {
    img {
      position: relative;
      height: 100%;
      object-fit: cover;
      opacity: 0.6; /* серебристый эффект */
    }
  }
  .vignette-left {
    left: -5vw;
    filter: drop-shadow(0 0 5px silver);
    transform: scaleX(-1);
  }

  .vignette-right {
    right: -5vw;
    filter: drop-shadow(0 0 5px silver);

  }
}

@media (max-width: 1535px) {
  .vignette {
    img {
      position: relative;
      height: 100%;
      object-fit: cover;
      opacity: 0.6; /* серебристый эффект */
    }
  }
  .vignette-left {
    left: -5vw;
    filter: drop-shadow(0 0 5px silver);
    transform: scaleX(-1);
  }

  .vignette-right {
    right: -5vw;
    filter: drop-shadow(0 0 5px silver);

  }
}

@media (max-width: 1400px) {
  .vignette {
    img {
      position: relative;
      height: 100%;
      object-fit: cover;
      opacity: 0.6; /* серебристый эффект */
    }
  }
  .vignette-left {
    left: -5vw;
    filter: drop-shadow(0 0 5px silver);
    transform: scaleX(-1);
  }

  .vignette-right {
    right: -5vw;
    filter: drop-shadow(0 0 5px silver);

  }
}

@media (max-width: 1245px) {
  .vignette {
    width: 46vh;
    img {
      position: relative;
      height: 100%;
      object-fit: cover;
      opacity: 0.6; /* серебристый эффект */
    }
  }
  .vignette-left {
    left: -5vw;
    filter: drop-shadow(0 0 5px silver);
    transform: scaleX(-1);
  }

  .vignette-right {
    right: -5vw;
    filter: drop-shadow(0 0 5px silver);

  }
}

@media (max-width: 1110px) {
  .vignette {
    width: 10vh;
    img {
      position: relative;
      width: 225px;
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

/* Мобильная адаптация */
@media (max-width: 1150px) {
  .vignette {
    display: none;
  }
}


.about-content {
  max-width: 1000px;
  margin: 0 auto;
  padding: 40px 60px;
  background-color: rgba(20, 20, 20, 0.8);
  border-radius: 16px;
  backdrop-filter: blur(4px);
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.5);
  animation: fadeInUp 1s ease forwards;
  position: relative;
  z-index: 2;
}

.about-title {
  font-size: 36px;
  font-weight: 700;
  margin-bottom: 30px;
  color: #ffffff;
  text-align: center;
  position: relative;
}

.about-title::after {
  content: '';
  display: block;
  width: 190px;
  height: 3px;
  background: linear-gradient(90deg, transparent, #adacac, transparent);
  margin: 15px auto 0;
}

.about-text p {
  font-size: 18px;
  line-height: 1.8;
  margin-bottom: 20px;
  color: #e0e0e0;
}

.about-text strong {
  color: #ffffff;
  font-weight: 600;
}

.highlight {
  font-size: 20px;
  color: #ff5e57;
  text-align: center;
  margin-top: 40px !important;
  font-style: italic;
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

.about-photo {
  margin-top: 60px;
  text-align: center;
}

.studio-title {
  font-size: 28px;
  margin-bottom: 30px;
  color: #ffffff;
  font-weight: 600;
}

.studio-title::after {
  content: '';
  display: block;
  width: 500px;
  height: 2px;
  background: linear-gradient(90deg, transparent, #adacac, transparent);
  margin: 10px auto 0;
}

.studio-gallery {
  display: flex;
  justify-content: center;
  gap: 20px;
  flex-wrap: wrap;
}

.studio-gallery img {
  width: 246px;
  height: auto;
  border-radius: 12px;
  object-fit: cover;
  transition: transform 0.3s ease;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
}

.studio-gallery img:hover {
  transform: scale(1.05);
}

/* Большие экраны (≥1921px) */
@media (min-width: 1921px) {
  .studio-gallery {
    gap: 30px;
    justify-content: center;
  }

  .studio-gallery img {
    width: 390px;
    height: 520px;
    max-width: 100%;
    border-radius: 12px;
  }

  .about-content {
    max-width: 1400px;
  }

  .about-title {
    font-size: 45px;
  }

  .studio-title {
    font-size: 35px;
    margin-bottom: 30px;
    color: #ffffff;
    font-weight: 600;
  }

  .about-text p {
    font-size: 25px;
    line-height: 1.8;
    margin-bottom: 20px;
    color: #e0e0e0;
  }

  .vignette {
    img {
      position: relative;
      width: 700px;
      height: 100%;
      object-fit: cover;
      opacity: 0.6; /* серебристый эффект */
    }
  }
  .vignette-left {
    left: -1vw;
    filter: drop-shadow(0 0 5px silver);
    transform: scaleX(-1);
  }

  .vignette-right {
    right: -1vw;
    filter: drop-shadow(0 0 5px silver);

  }
}

/* Ноутбуки (1024px–1920px) */
@media (min-width: 1025px) and (max-width: 1920px) {
  .studio-gallery {
    gap: 20px;
  }

  .studio-gallery img {
    width: calc(33.333% - 13.33px);
    max-width: 100%;
    height: 473px;
  }
}

/* Планшеты (768px–1024px) */
@media (max-width: 1024px) {
  .studio-title {
    font-size: 24px;
  }

  .studio-title::after {
    width: 260px;
  }

  .studio-gallery {
    gap: 30px;
  }

  .studio-gallery img {
    width: calc(50% - 164px);
    max-width: 100%;
    height: 344px;
  }
}

/* Мобильные (481px–768px) */
@media (max-width: 768px) {
  .studio-title {
    font-size: 22px;
  }

  .studio-title::after {
    width: 220px;
  }

  .studio-gallery {
    gap: 12px;
    justify-content: center;
  }

  .studio-gallery img {
    width: 298px;
    height: 397.33px;
    border-radius: 10px;
  }
}

/* Очень маленькие экраны (до 480px) */
@media (max-width: 480px) {
  .container {
    padding: 0 10px;
  }

  .about-content {
    padding: 20px;
  }

  .about-title {
    font-size: 25px;
    margin-bottom: 20px;
  }

  .about-text p {
    font-size: 17px;
    line-height: 1.8;
    margin-bottom: 17px;
    color: #e0e0e0;
  }

  .studio-title {
    font-size: 20px;
  }

  .studio-title::after {
    width: 300px;
  }

  .studio-gallery {
    gap: 10px;
    justify-content: center;
  }

  .studio-gallery img {
    width: clamp(145px, 40vw, 240px);
    height: 193.33px;
    border-radius: 10px;
  }
}

</style>