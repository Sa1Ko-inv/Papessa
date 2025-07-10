<template>
  <div>
    <!-- Кнопка открытия -->
    <button @click="isOpen = true" class="open-button">Получить консультацию</button>

    <!-- Модальное окно -->
    <div v-if="isOpen" class="modal-overlay" @click="close">
      <div class="modal" @click.stop>
        <h2>Запись на консультацию</h2>

        <input v-model="name" type="text" placeholder="Ваше имя" />
        <input v-model="phone" type="tel" placeholder="Номер телефона"/>
        <input v-model="tg" type="text" placeholder="Ваш телеграм (Если есть)"/>
        <input v-model="vk" type="text" placeholder="Ваша ВК (Если есть)"/>
        <input v-model="date" type="date" />
        <select v-model="time">
          <option disabled value="">Выберите время</option>
          <option value="Утро">Утро</option>
          <option value="День">День</option>
          <option value="Вечер">Вечер</option>
        </select>

        <div class="modal-actions">
          <button :disabled="loading" @click="submit">
            {{ loading ? 'Отправка...' : 'Записаться' }}
          </button>
          <button @click="close">Отмена</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const isOpen = ref(false)
const name = ref('')
const phone = ref('')
const tg = ref('')
const vk = ref('')
const date = ref('')
const time = ref('')
const loading = ref(false)

const close = () => {
  isOpen.value = false
  name.value = ''
  phone.value = ''
  tg.value = ''
  vk.value = ''
  date.value = ''
  time.value = ''
}

const submit = async () => {
  if (!name.value || !phone.value || !date.value || !time.value) {
    alert('Пожалуйста, заполните все поля.')
    return
  }

  const message = `💬 Новая заявка на консультацию:
👤 Имя: ${name.value}
📞 Телефон: ${phone.value}
✈️ Тг: ${tg.value}
🔵 ВК: ${vk.value}
📅 Дата: ${date.value}
🕒 Время суток: ${time.value}`

  const token = import.meta.env.VITE_TG_TOKEN
  const chatId = import.meta.env.VITE_TG_CHAT_ID

  loading.value = true
  try {
    await fetch(`https://api.telegram.org/bot${token}/sendMessage`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ chat_id: chatId, text: message }),
    })

    alert('Заявка успешно отправлена!')
    close()
  } catch (error) {
    alert('Ошибка при отправке. Попробуйте позже.')
    console.error(error)
  } finally {
    loading.value = false
  }
}
</script>

<style scoped>
/* === КНОПКА "Получить консультацию" — АДАПТИВНЫЕ СТИЛИ === */

.open-button {
  padding: 14px 28px;
  font-size: clamp(15px, 1.2vw, 18px);
  background-color: #d7d7d7;
  color: black;
  filter: drop-shadow(0 0 5px silver);
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
}
.open-button:hover {
  background-color: #c2c2c2;
}

/* Большие экраны (≥1920px) */
@media (min-width: 1920px) {
  .open-button {
    padding: 18px 36px;
    font-size: 20px;
    border-radius: 10px;
  }
}

/* Ноутбуки (1024px–1919px) */
@media (min-width: 1024px) and (max-width: 1919px) {
  .open-button {
    padding: 14px 28px;
    font-size: 18px;
  }
}

/* Планшеты (768px–1023px) */
@media (max-width: 1023px) {
  .open-button {
    padding: 12px 24px;
    font-size: 17px;
  }
}

/* Мобильные (481px–767px) */
@media (max-width: 767px) {
  .open-button {
    padding: 10px 22px;
    font-size: 16px;
  }
}

/* Маленькие мобильные (до 480px) */
@media (max-width: 480px) {
  .open-button {
    width: 100%;
    padding: 12px 0;
    font-size: 16px;
    border-radius: 6px;
    margin-top: 15px;
  }
}

.modal-overlay {
  position: fixed;
  top: -18vh;
  left: -40vw;
  width: 150vw;
  height: 110vh;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10000;
}

@media (max-width: 1650px) {
  .modal-overlay {
    top: -15vh;
    left: -20vw;
    width: 120vw;
    height: 110vh;
  }
}

@media (max-width: 1400px) {
  .modal-overlay {
    top: -17vh;
    left: -5vw;
    width: 100vw;
    height: 110vh;
  }
}

.modal {
  background: #1e1e1e;
  padding: 30px;
  border-radius: 16px;
  width: 440px;
  color: white;
  display: flex;
  flex-direction: column;
  gap: 14px;
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.6);
}

.modal h2 {
  margin-bottom: 10px;
  font-size: 22px;
  color: white;
}

.modal input,
.modal select {
  padding: 10px;
  border: none;
  border-radius: 8px;
  background: #2c2c2c;
  color: white;
  font-size: 14px;
}

.modal input::placeholder {
  color: #aaa;
}

.modal select {
  color: white;
}

.modal-actions {
  display: flex;
  justify-content: space-between;
  gap: 10px;
  margin-top: 10px;
}

.modal-actions button {
  padding: 10px 16px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  flex: 1;
  transition: background-color 0.3s;
}

.modal-actions button:first-child {
  background: #4caf50;
  color: white;
}
.modal-actions button:first-child:disabled {
  background: #7cbf7c;
  cursor: not-allowed;
}

.modal-actions button:last-child {
  background: #666;
  color: white;
}
.modal-actions button:last-child:hover {
  background: #888;
}
</style>
