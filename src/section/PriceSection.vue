<template>
  <section
      class="price-section"
      id="price"
      ref="aboutSection"
      :style="{ paddingTop: sectionPadding, paddingBottom: sectionPadding }"
  >
    <div class="vignette vignette-left"><img src="./../assets/V1L.png" alt=""></div>
    <div class="vignette vignette-right"><img src="./../assets/V1L.png" alt=""></div>
    <div class="container">
      <h2 class="section-title">Узнай цену на татуировку</h2>
      <p class="description">
        Цены на татуировки зависят от сложности работы, размера и места. Мы предлагаем индивидуальный подход к каждому клиенту.
        Для получения точной информации о стоимости, пожалуйста, свяжитесь с нами через форму ниже.
      </p>

      <form @submit.prevent="submit" class="form">
        <label for="name">Имя</label>
        <input id="name" v-model="name" type="text" required />

        <label for="phone">Телефон</label>
        <input id="phone" v-model="phone" type="tel" required />

        <label for="tg">Ваш телеграм (Если есть)</label>
        <input id="tg" v-model="tg" type="text" />

        <label for="vk">Ваш ВК (Если есть)</label>
        <input id="vk" v-model="vk" type="text" />

        <label for="size">Размер тату в см</label>
        <input id="size" type="number" v-model="size"/>

        <label for="location">Место:</label>
        <input id="location" type="text" v-model="location">

        <label for="file" class="file-label">📎 Прикрепить эскизы (png, jpg, jpeg)</label>
        <input
            id="file"
            type="file"
            multiple
            @change="handleFile"
            accept="image/png, image/jpeg, image/jpg"
            ref="fileInput"
            class="hidden-file-input"
        />

        <div v-if="files.length" class="file-list">
          <p>Загруженные файлы:</p>
          <ul>
            <li v-for="(file, index) in files" :key="index">
              {{ file.name }}
              <button type="button" @click="removeFile(index)" class="remove-btn">✖</button>
            </li>
          </ul>
        </div>

        <button :disabled="loading">
          {{ loading ? 'Отправка...' : 'Узнать цену' }}
        </button>
      </form>
    </div>
  </section>
</template>

<script setup>
import {onMounted, ref} from 'vue'

const name = ref('')
const phone = ref('')
const tg = ref('')
const vk = ref('')
const size = ref('')
const location = ref('')
const files = ref([])
const fileInput = ref(null)
const loading = ref(false)

const handleFile = (e) => {
  const newFiles = Array.from(e.target.files);
  files.value = [...files.value, ...newFiles];
}

const resetForm = () => {
  name.value = ''
  phone.value = ''
  tg.value = ''
  vk.value = ''
  size.value = ''
  location.value = ''
  files.value = []
  if (fileInput.value) {
    fileInput.value.value = '' // Сброс input[type=file]
  }
}

const removeFile = (index) => {
  files.value.splice(index, 1);
}

const submit = async () => {
  if (!name.value || !phone.value) {
    alert('Пожалуйста, заполните имя и телефон.')
    return
  }

  const token = import.meta.env.VITE_TG_TOKEN
  const chatId = import.meta.env.VITE_TG_CHAT_ID

  const text = `💬 Новая заявка на цену:
👤 Имя: ${name.value}
📞 Телефон: ${phone.value}
✈️ Тг: ${tg.value}
🔵 ВК: ${vk.value}
📏 Размер: ${size.value}
📍 Место: ${location.value}`

  loading.value = true

  try {
    // Отправляем сообщение
    await fetch(`https://api.telegram.org/bot${token}/sendMessage`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ chat_id: chatId, text })
    })

    // Если есть файл — отправляем его
    if (files.value.length > 0) {
      for (const image of files.value) {
        const formData = new FormData()
        formData.append('chat_id', chatId)
        formData.append('photo', image)
        formData.append('caption', 'Эскиз от клиента')

        await fetch(`https://api.telegram.org/bot${token}/sendPhoto`, {
          method: 'POST',
          body: formData
        })
      }
    }

    alert('Заявка успешно отправлена!')
    resetForm()
  } catch (e) {
    console.error(e)
    alert('Ошибка при отправке. Попробуйте позже.')
  } finally {
    loading.value = false
  }
}

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
.price-section {
  position: relative;
  background-color: #121212;
  color: #f0f0f0;
  padding: 80px 20px;
  opacity: 0;
  transform: scale(0.95);
  transition: opacity 0.6s ease, transform 0.6s ease;
}

.price-section.visible {
  opacity: 1;
  transform: scale(1);
}

.price-section::before {
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
  width: 500px; /* регулируй по желанию */
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
  left: -5.4vh;
  filter: drop-shadow(0 0 5px silver);
  transform: scaleX(-1);

}

.vignette-right {
  right: -5.4vh;
  filter: drop-shadow(0 0 5px silver);

}

@media (max-width: 1600px) {
  .vignette {
    width: 390px;
    img {
      position: relative;
      height: 100%;
      object-fit: cover;
      opacity: 0.6; /* серебристый эффект */
      transform: scaleY(-1);
    }
  }
  .vignette-left {
    left: -5.4vh;
    filter: drop-shadow(0 0 5px silver);
    transform: scaleX(-1);

  }

  .vignette-right {
    right: -5.4vh;
    filter: drop-shadow(0 0 5px silver);

  }
}

@media (max-width: 1450px) {
  .vignette {
    width: 320px;
    img {
      position: relative;
      height: 100%;
      object-fit: cover;
      opacity: 0.6; /* серебристый эффект */
      transform: scaleY(-1);
    }
  }
  .vignette-left {
    left: -5.4vh;
    filter: drop-shadow(0 0 5px silver);
    transform: scaleX(-1);

  }

  .vignette-right {
    right: -5.4vh;
    filter: drop-shadow(0 0 5px silver);

  }
}

@media (max-width: 1245px) {
  .vignette {
  width: 39vh;
    img {
      position: relative;
      width: 100%;
      object-fit: cover;
      opacity: 0.6; /* серебристый эффект */
    }
  }
  .vignette-left {
    left: -22vh;
    filter: drop-shadow(0 0 5px silver);
    transform: scaleX(-1);
  }

  .vignette-right {
    right: -22vh;
    filter: drop-shadow(0 0 5px silver);

  }
}
@media (max-width: 1375px) {
  .vignette {
    width: 39vh;
    img {
      position: relative;
      width: 100%;
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
    width: 300px;
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
    width: 320px;
    img {
      position: relative;
      width: 100%;
      object-fit: cover;
      opacity: 0.6; /* серебристый эффект */
    }
  }
  .vignette-left {
    left: -19vh;
    filter: drop-shadow(0 0 5px silver);
    transform: scaleX(-1);
  }

  .vignette-right {
    right: -19vh;
    filter: drop-shadow(0 0 5px silver);

  }
}

/* Мобильная адаптация */
@media (max-width: 1150px) {
  .vignette {
    display: none;
  }
}


.container {
  max-width: 900px;
  margin: 0 auto;
  background: rgba(20, 20, 20, 0.85);
  padding: 40px;
  border-radius: 16px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.6);
}

.section-title {
  font-size: 32px;
  text-align: center;
  margin-bottom: 24px;
  color: #ffffff;
  position: relative;
}

.section-title::after {
  content: '';
  display: block;
  width: 500px;
  height: 3px;
  background: linear-gradient(90deg, transparent, #adacac, transparent);
  margin: 16px auto 0;
}

.description {
  text-align: center;
  margin-bottom: 32px;
  font-size: 19px;
  color: #cccccc;
}

.form {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

input,
button {
  padding: 12px;
  border-radius: 8px;
  border: none;
  font-size: 16px;
}

input {
  background: #1e1e1e;
  color: #f0f0f0;
  border: 1px solid #333;
}

input:focus {
  outline: none;
  border-color: #adacac;
}

button {
  background-color: #d7d7d7;
  filter: drop-shadow(0 0 5px silver);
  color: black;
  cursor: pointer;
  transition: background 0.3s;
}

button:hover:not(:disabled) {
  background-color: #c2c2c2;
}

button:disabled {
  background-color: #666;
  cursor: not-allowed;
}

.file-list {
  background: #1a1a1a;
  padding: 10px;
  border-radius: 8px;
  margin-top: -8px;
  margin-bottom: 8px;
}

.file-list ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.file-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 6px 0;
  border-bottom: 1px solid #333;
  color: #f0f0f0;
  font-size: 14px;
}

.remove-btn {
  background: transparent;
  border: none;
  color: #ff5e57;
  font-size: 16px;
  cursor: pointer;
  padding: 0 8px;
}
.remove-btn:hover {
  color: #ff2e2e;
}


/* Responsive */
@media (max-width: 600px) {
  .container {
    padding: 20px;
  }

  .section-title {
    font-size: 24px;
  }

  input,
  button {
    font-size: 14px;
    padding: 10px;
  }
}

.hidden-file-input {
  display: none;
}

.file-label {
  margin-top: 15px;
  display: inline-block;
  padding: 12px 16px;
  background-color: #2c2c2c;
  color: #f0f0f0;
  border: 1px dashed #555;
  border-radius: 8px;
  cursor: pointer;
  text-align: center;
  transition: all 0.3s ease;
  font-size: 14px;
}

.file-label:hover {
  background-color: #333;
  border-color: #adacac;
  color: #adacac;
}

</style>


<!--<template>-->
<!--  <section-->
<!--      class="price-section"-->
<!--      id="price"-->
<!--      ref="aboutSection"-->
<!--      :style="{ paddingTop: sectionPadding, paddingBottom: sectionPadding }"-->
<!--  >-->
<!--    <div class="vignette vignette-left"><img src="./../assets/V1L.png" alt=""></div>-->
<!--    <div class="vignette vignette-right"><img src="./../assets/V1L.png" alt=""></div>-->
<!--    <div class="container">-->
<!--      <h2 class="section-title">Узнай цену на татуировку</h2>-->
<!--      <p class="description">-->
<!--        Цены на татуировки зависят от сложности работы, размера и места. Мы предлагаем индивидуальный подход к каждому клиенту.-->
<!--        Для получения точной информации о стоимости, пожалуйста, свяжитесь с нами через форму ниже.-->
<!--      </p>-->

<!--      <form @submit.prevent="submit" class="form">-->
<!--        <label for="name">Имя</label>-->
<!--        <input id="name" v-model="name" type="text" required />-->

<!--        <label for="phone">Телефон</label>-->
<!--        <input id="phone" v-model="phone" type="tel" required />-->

<!--        <label for="tg">Ваш телеграм (Если есть)</label>-->
<!--        <input id="tg" v-model="tg" type="text" />-->

<!--        <label for="vk">Ваш ВК (Если есть)</label>-->
<!--        <input id="vk" v-model="vk" type="text" />-->

<!--        <label for="size">Размер тату в см</label>-->
<!--        <input id="size" type="number" v-model="size"/>-->

<!--        <label for="location">Место:</label>-->
<!--        <input id="location" type="text" v-model="location">-->

<!--        <label for="file" class="file-label">📎 Прикрепить эскизы (png, jpg, jpeg)</label>-->
<!--        <input-->
<!--            id="file"-->
<!--            type="file"-->
<!--            multiple-->
<!--            @change="handleFile"-->
<!--            accept="image/png, image/jpeg, image/jpg"-->
<!--            ref="fileInput"-->
<!--            class="hidden-file-input"-->
<!--        />-->

<!--        <div v-if="files.length" class="file-list">-->
<!--          <p>Загруженные файлы:</p>-->
<!--          <ul>-->
<!--            <li v-for="(file, index) in files" :key="index">-->
<!--              {{ file.name }}-->
<!--              <button type="button" @click="removeFile(index)" class="remove-btn">✖</button>-->
<!--            </li>-->
<!--          </ul>-->
<!--        </div>-->

<!--        <button :disabled="loading">-->
<!--          {{ loading ? 'Отправка...' : 'Узнать цену' }}-->
<!--        </button>-->
<!--      </form>-->
<!--    </div>-->
<!--  </section>-->
<!--</template>-->

<!--<script setup>-->
<!--import {onMounted, ref} from 'vue'-->

<!--const name = ref('')-->
<!--const phone = ref('')-->
<!--const tg = ref('')-->
<!--const vk = ref('')-->
<!--const size = ref('')-->
<!--const location = ref('')-->
<!--const files = ref([])-->
<!--const fileInput = ref(null)-->
<!--const loading = ref(false)-->

<!--const handleFile = (e) => {-->
<!--  const newFiles = Array.from(e.target.files);-->
<!--  files.value = [...files.value, ...newFiles];-->
<!--}-->

<!--const resetForm = () => {-->
<!--  name.value = ''-->
<!--  phone.value = ''-->
<!--  tg.value = ''-->
<!--  vk.value = ''-->
<!--  size.value = ''-->
<!--  location.value = ''-->
<!--  files.value = []-->
<!--  if (fileInput.value) {-->
<!--    fileInput.value.value = '' // Сброс input[type=file]-->
<!--  }-->
<!--}-->

<!--const removeFile = (index) => {-->
<!--  files.value.splice(index, 1);-->
<!--}-->

<!--const submit = async () => {-->
<!--  if (!name.value || !phone.value) {-->
<!--    alert('Пожалуйста, заполните имя и телефон.')-->
<!--    return-->
<!--  }-->

<!--  const token = import.meta.env.VITE_TG_TOKEN-->
<!--  const chatId = import.meta.env.VITE_TG_CHAT_ID-->

<!--  const text = `💬 Новая заявка на цену:-->
<!--👤 Имя: ${name.value}-->
<!--📞 Телефон: ${phone.value}-->
<!--✈️ Тг: ${tg.value}-->
<!--🔵 ВК: ${vk.value}-->
<!--📏 Размер: ${size.value}-->
<!--📍 Место: ${location.value}`-->

<!--  loading.value = true-->

<!--  try {-->
<!--    // Отправляем сообщение-->
<!--    await fetch(`https://api.telegram.org/bot${token}/sendMessage`, {-->
<!--      method: 'POST',-->
<!--      headers: { 'Content-Type': 'application/json' },-->
<!--      body: JSON.stringify({ chat_id: chatId, text })-->
<!--    })-->

<!--    // Если есть файл — отправляем его-->
<!--    if (files.value.length > 0) {-->
<!--      for (const image of files.value) {-->
<!--        const formData = new FormData()-->
<!--        formData.append('chat_id', chatId)-->
<!--        formData.append('photo', image)-->
<!--        formData.append('caption', 'Эскиз от клиента')-->

<!--        await fetch(`https://api.telegram.org/bot${token}/sendPhoto`, {-->
<!--          method: 'POST',-->
<!--          body: formData-->
<!--        })-->
<!--      }-->
<!--    }-->

<!--    alert('Заявка успешно отправлена!')-->
<!--    resetForm()-->
<!--  } catch (e) {-->
<!--    console.error(e)-->
<!--    alert('Ошибка при отправке. Попробуйте позже.')-->
<!--  } finally {-->
<!--    loading.value = false-->
<!--  }-->
<!--}-->

<!--const aboutSection = ref(null);-->
<!--const sectionPadding = ref('100px');-->
<!--// Для точного позиционирования при скролле-->
<!--onMounted(() => {-->
<!--  // Рассчитываем отступ с учетом высоты навбара-->
<!--  const navbarHeight = document.querySelector('.navbar-wrapper').offsetHeight;-->
<!--  sectionPadding.value = `${navbarHeight + 30}px`;-->

<!--  // Наблюдатель для анимации при скролле-->
<!--  const observer = new IntersectionObserver((entries) => {-->
<!--    entries.forEach(entry => {-->
<!--      if (entry.isIntersecting) {-->
<!--        entry.target.classList.add('visible');-->
<!--      }-->
<!--    });-->
<!--  }, { threshold: 0.1 });-->

<!--  if (aboutSection.value) {-->
<!--    observer.observe(aboutSection.value);-->
<!--  }-->
<!--});-->
<!--</script>-->

<!--<style scoped>-->
<!--.price-section {-->
<!--  position: relative;-->
<!--  background-color: #121212;-->
<!--  color: #f0f0f0;-->
<!--  padding: 80px 20px;-->
<!--  opacity: 0;-->
<!--  transform: scale(0.95);-->
<!--  transition: opacity 0.6s ease, transform 0.6s ease;-->
<!--}-->

<!--.price-section.visible {-->
<!--  opacity: 1;-->
<!--  transform: scale(1);-->
<!--}-->

<!--.price-section::before {-->
<!--  content: '';-->
<!--  position: absolute;-->
<!--  top: 0;-->
<!--  left: 0;-->
<!--  right: 0;-->
<!--  height: 2px;-->
<!--  background: linear-gradient(90deg, transparent, #ff5e57, transparent);-->
<!--}-->

<!--.vignette {-->
<!--  position: absolute;-->
<!--  top: 0;-->
<!--  bottom: 0;-->
<!--  width: 500px; /* регулируй по желанию */-->
<!--  background-repeat: repeat-x;-->
<!--  background-size: contain;-->
<!--  background-position: center;-->
<!--  z-index: 1; /* под контентом, но выше фона */-->
<!--  opacity: 0.6; /* серебристый эффект */-->
<!--  pointer-events: none;-->


<!--  img {-->
<!--    position: relative;-->
<!--    width: 500px;-->
<!--    height: 100%;-->
<!--    object-fit: cover;-->
<!--    opacity: 0.6; /* серебристый эффект */-->
<!--    transform: scaleY(-1);-->
<!--  }-->
<!--}-->

<!--.vignette-left {-->
<!--  left: -10vh;-->
<!--  filter: drop-shadow(0 0 5px silver);-->
<!--  transform: scaleX(-1);-->

<!--}-->

<!--.vignette-right {-->
<!--  right: -10vh;-->
<!--  filter: drop-shadow(0 0 5px silver);-->

<!--}-->

<!--/* Мобильная адаптация */-->
<!--@media (max-width: 768px) {-->
<!--  .vignette {-->
<!--    display: none;-->
<!--  }-->
<!--}-->

<!--.container {-->
<!--  max-width: 900px;-->
<!--  margin: 0 auto;-->
<!--  background: rgba(20, 20, 20, 0.85);-->
<!--  padding: 40px;-->
<!--  border-radius: 16px;-->
<!--  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.6);-->
<!--}-->

<!--.section-title {-->
<!--  font-size: 32px;-->
<!--  text-align: center;-->
<!--  margin-bottom: 24px;-->
<!--  color: #ffffff;-->
<!--  position: relative;-->
<!--}-->

<!--.section-title::after {-->
<!--  content: '';-->
<!--  display: block;-->
<!--  width: 80px;-->
<!--  height: 3px;-->
<!--  background: #ff5e57;-->
<!--  margin: 16px auto 0;-->
<!--}-->

<!--.description {-->
<!--  text-align: center;-->
<!--  margin-bottom: 32px;-->
<!--  font-size: 19px;-->
<!--  color: #cccccc;-->
<!--}-->

<!--.form {-->
<!--  display: flex;-->
<!--  flex-direction: column;-->
<!--  gap: 16px;-->
<!--}-->

<!--input,-->
<!--button {-->
<!--  padding: 12px;-->
<!--  border-radius: 8px;-->
<!--  border: none;-->
<!--  font-size: 16px;-->
<!--}-->

<!--input {-->
<!--  background: #1e1e1e;-->
<!--  color: #f0f0f0;-->
<!--  border: 1px solid #333;-->
<!--}-->

<!--input:focus {-->
<!--  outline: none;-->
<!--  border-color: #ff5e57;-->
<!--}-->

<!--button {-->
<!--  background-color: #ff5e57;-->
<!--  color: white;-->
<!--  cursor: pointer;-->
<!--  transition: background 0.3s;-->
<!--}-->

<!--button:hover:not(:disabled) {-->
<!--  background-color: #e14c47;-->
<!--}-->

<!--button:disabled {-->
<!--  background-color: #666;-->
<!--  cursor: not-allowed;-->
<!--}-->

<!--.file-list {-->
<!--  background: #1a1a1a;-->
<!--  padding: 10px;-->
<!--  border-radius: 8px;-->
<!--  margin-top: -8px;-->
<!--  margin-bottom: 8px;-->
<!--}-->

<!--.file-list ul {-->
<!--  list-style: none;-->
<!--  padding: 0;-->
<!--  margin: 0;-->
<!--}-->

<!--.file-list li {-->
<!--  display: flex;-->
<!--  justify-content: space-between;-->
<!--  align-items: center;-->
<!--  padding: 6px 0;-->
<!--  border-bottom: 1px solid #333;-->
<!--  color: #f0f0f0;-->
<!--  font-size: 14px;-->
<!--}-->

<!--.remove-btn {-->
<!--  background: transparent;-->
<!--  border: none;-->
<!--  color: #ff5e57;-->
<!--  font-size: 16px;-->
<!--  cursor: pointer;-->
<!--  padding: 0 8px;-->
<!--}-->
<!--.remove-btn:hover {-->
<!--  color: #ff2e2e;-->
<!--}-->


<!--/* Responsive */-->
<!--@media (max-width: 600px) {-->
<!--  .container {-->
<!--    padding: 20px;-->
<!--  }-->

<!--  .section-title {-->
<!--    font-size: 24px;-->
<!--  }-->

<!--  input,-->
<!--  button {-->
<!--    font-size: 14px;-->
<!--    padding: 10px;-->
<!--  }-->
<!--}-->

<!--.hidden-file-input {-->
<!--  display: none;-->
<!--}-->

<!--.file-label {-->
<!--  margin-top: 15px;-->
<!--  display: inline-block;-->
<!--  padding: 12px 16px;-->
<!--  background-color: #2c2c2c;-->
<!--  color: #f0f0f0;-->
<!--  border: 1px dashed #555;-->
<!--  border-radius: 8px;-->
<!--  cursor: pointer;-->
<!--  text-align: center;-->
<!--  transition: all 0.3s ease;-->
<!--  font-size: 14px;-->
<!--}-->

<!--.file-label:hover {-->
<!--  background-color: #333;-->
<!--  border-color: #ff5e57;-->
<!--  color: #ff5e57;-->
<!--}-->

<!--</style>-->
