<template>
  <section
      id="work"
      class="work"
      ref="aboutSection"
      :style="{ paddingTop: sectionPadding, paddingBottom: sectionPadding }"
  >
    <div class="vignette vignette-left"><img src="./../assets/V1L.png" alt=""></div>
    <div class="vignette vignette-right"><img src="./../assets/V1L.png" alt=""></div>
    <div class="container">
      <h2 class="work-title">Работы наших мастеров</h2>
      <div class="work-dariya">
          <h2>Работы Дарьи</h2>
        <swiper
            :slidesPerView="3"
            :spaceBetween="30"
            :pagination="{clickable: true,}"
            :modules="modules"
            class="mySwiper"
            style="--swiper-pagination-color: white; --swiper-pagination-bullet-inactive-color: rgba(255,255,255,0.9);"
        >
          <swiper-slide><img src="./../assets/D-Rab-1.jpg" alt="D-Rab-1"></swiper-slide>
          <swiper-slide><img src="./../assets/D-Rab-2.jpg" alt="D-Rab-2"></swiper-slide>
          <swiper-slide><img src="./../assets/D-Rab-3.jpg" alt="D-Rab-3"></swiper-slide>
          <swiper-slide><img src="./../assets/D-Rab-4.jpg" alt="D-Rab-4"></swiper-slide>
          <swiper-slide><img src="./../assets/D-Rab-5.jpg" alt="D-Rab-5"></swiper-slide>
          <swiper-slide><img src="./../assets/D-Rab-6.jpg" alt="D-Rab-6"></swiper-slide>
          <swiper-slide><img src="./../assets/D-Rab-7.jpg" alt="D-Rab-7"></swiper-slide>
        </swiper>
      </div>

      <div class="work-sonya">
        <h2>Работы Софьи</h2>
        <swiper
            :slidesPerView="3"
            :spaceBetween="30"
            :pagination="{clickable: true,}"
            :modules="modules"
            class="mySwiper"
            style="--swiper-pagination-color: white; --swiper-pagination-bullet-inactive-color: rgba(255,255,255,0.9);"
        >
          <swiper-slide><img src="./../assets/S-Rab-1.jpg" alt="S-Rab-1"></swiper-slide>
          <swiper-slide><img src="./../assets/S-Rab-2.jpg" alt="S-Rab-2"></swiper-slide>
          <swiper-slide><img src="./../assets/S-Rab-3.jpg" alt="S-Rab-3"></swiper-slide>
          <swiper-slide><img src="./../assets/S-Rab-4.jpg" alt="S-Rab-4"></swiper-slide>
        </swiper>
      </div>
    </div>
  </section>
</template>

<script setup>
import {ref, onMounted} from 'vue';
import {Swiper, SwiperSlide} from 'swiper/vue';
import {Pagination} from 'swiper/modules';
import 'swiper/css';
import 'swiper/css/pagination';


const aboutSection = ref(null);
const sectionPadding = ref('100px');
const modules = [Pagination];

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
  }, {threshold: 0.1});

  if (aboutSection.value) {
    observer.observe(aboutSection.value);
  }
});


</script>

<style scoped>
.work {
  position: relative;
  padding: 80px 0;
  background-color: #121212;
  color: #f0f0f0;
  opacity: 0;
  transform: scale(0.95);
  transition: opacity 0.6s ease, transform 0.6s ease;
}

.work.visible {
  opacity: 1;
  transform: scale(1);
}

.work::before {
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
  background-repeat: repeat-x;
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
  left: -6vw;
  filter: drop-shadow(0 0 5px silver);
  transform: scaleX(-1);

}

.vignette-right {
  right: -6vw;
  filter: drop-shadow(0 0 5px silver);

}

@media (max-width: 1670px) {
  .vignette {
    img {
      position: relative;
      height: 100%;
      object-fit: cover;
      opacity: 0.6; /* серебристый эффект */
      transform: scaleY(-1);
      z-index: 1;
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

@media (max-width: 1450px) {
  .vignette {
    img {
      position: relative;
      height: 100%;
      object-fit: cover;
      opacity: 0.6; /* серебристый эффект */
      transform: scaleY(-1);
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

@media (max-width: 1375px) {
  .vignette {
    img {
      position: relative;
      height: 100%;
      object-fit: cover;
      opacity: 0.6; /* серебристый эффект */
    }
  }
  .vignette-left {
    left: -15vh;
    filter: drop-shadow(0 0 5px silver);
    transform: scaleX(-1);
  }

  .vignette-right {
    right: -15vh;
    filter: drop-shadow(0 0 5px silver);

  }
}

@media (max-width: 1295px) {
  .vignette {
    img {
      position: relative;
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
}

@media (max-width: 1200px) {
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

/* Мобильная адаптация */
@media (max-width: 1150px) {
  .vignette {
    display: none;
  }
}

.work-title {
  font-size: 36px;
  font-weight: 700;
  margin-bottom: 40px;
  color: #ffffff;
  text-align: center;
  position: relative;
}

.work-title::after {
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

.work-dariya,
.work-sonya {
  margin-top: 60px;
}

.work-dariya h2,
.work-sonya h2 {
  font-size: 28px;
  color: #f5f5f5;
  margin-bottom: 20px;
  position: relative;
  text-align: left;
}

.work-dariya h2::before,
.work-sonya h2::before {
  content: '';
  position: absolute;
  left: 0;
  bottom: -6px;
  width: 200px;
  height: 3px;
  background: #adacac;
  border-radius: 2px;
}

/* Можно добавить небольшие различия между Дарьей и Софьей (по желанию) */
.work-dariya h2::before {
  background: linear-gradient(90deg, #60a5fa, transparent);; /* светло-фиолетовая */
}

.work-sonya h2::before {
  background: linear-gradient(90deg, #c084fc, transparent);; /* светло-синяя */
}


.mySwiper {
  margin-bottom: 60px;

  .swiper-slide {
    background-color: #1e1e1e;
    border-radius: 12px;
    overflow: hidden;
    text-align: center;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
    transition: transform 0.3s ease;

    img {
      width: 100%;
      height: 425px;
      object-fit: cover;
      border-radius: 12px;
    }

    &:hover {
      transform: scale(1.03);
    }
  }

  .swiper-pagination-bullet {
    background: white;
    opacity: 0.7;
  }

  .swiper-pagination-bullet-active {
    background: #ffffff;
    opacity: 1;
  }
}

@media (max-width: 1024px) {
  .mySwiper {
    --swiper-pagination-bottom: 8px;
  }
}

@media (max-width: 768px) {
  .work-title {
    font-size: 28px;
  }

  .container {
    padding: 20px;
  }

  .mySwiper {
    .swiper-slide {
      height: auto;
    }
  }
}

@media (max-width: 640px) {
  .swiper {
    --swiper-pagination-bottom: 5px;
  }

  .swiper-slide {
    transform: none !important;
  }
}

</style>