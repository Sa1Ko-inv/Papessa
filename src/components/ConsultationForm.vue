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
        <Datepicker
            v-model="date"
            placeholder="Выберите дату"
            :dark="true"
            input-class-name="custom-date-input"
            :class="['custom-datepicker']"
            :locale="ru"
            :format="'dd/MM/yyyy'"
            :enable-time-picker="false"
            :auto-apply="false"
            :translations="datePickerTranslations"
        />
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
import Datepicker from '@vuepic/vue-datepicker'
import { ru } from 'date-fns/locale' // Правильный импорт
import '@vuepic/vue-datepicker/dist/main.css'

const datePickerTranslations = {
  cancel: 'Отмена',
  select: 'Выбрать',
  today: 'Сегодня',
};

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

.modal select {
  background: #2c2c2c url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' fill='white' viewBox='0 0 20 20'%3E%3Cpath d='M5.23 7.21a.75.75 0 011.06.02L10 10.94l3.71-3.71a.75.75 0 111.06 1.06l-4.24 4.24a.75.75 0 01-1.06 0L5.21 8.29a.75.75 0 01.02-1.08z'/%3E%3C/svg%3E") no-repeat right 1rem center / 1em !important;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  padding: 10px 2.5rem 10px 10px;
  border: none;
  border-radius: 8px;
  color: #aaa;
  font-size: 14px;
}

.modal input::placeholder {
  color: #aaa;
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


/* Большие экраны (≥1920px) */
@media (min-width: 1920px) {
  .open-button {
    padding: 18px 36px;
    font-size: 20px;
    border-radius: 10px;
  }

  .modal-overlay {
    top: -18vh;
    left: -25vw;
    width: 120vw;
    height: 110vh;
  }

  .modal {
    background: #1e1e1e;
    padding: 30px;
    border-radius: 16px;
    width: 42.97vw;
    height: 720px;
    color: white;
    display: flex;
    flex-direction: column;
    gap: 14px;
    box-shadow: 0 12px 30px rgba(0, 0, 0, 0.6);
  }

  .modal h2 {
    margin-bottom: 10px;
    font-size: 32px;
    color: white;
  }

  .modal input {
    padding: 25px;
    border: none;
    border-radius: 8px;
    background: #2c2c2c;
    color: white;
    font-size: 18px;
  }

  .modal select {
    background: #2c2c2c url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' fill='white' viewBox='0 0 20 20'%3E%3Cpath d='M5.23 7.21a.75.75 0 011.06.02L10 10.94l3.71-3.71a.75.75 0 111.06 1.06l-4.24 4.24a.75.75 0 01-1.06 0L5.21 8.29a.75.75 0 01.02-1.08z'/%3E%3C/svg%3E") no-repeat right 1.5rem center / 1.5em !important;
    padding: 25px 2.5rem 25px 25px;
    border-radius: 8px;
    font-size: 18px;
  }

  .modal-actions button {
    font-size: 22px;
    position: relative;
    top: 20%;
    padding: 20px 26px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    flex: 1;
    transition: background-color 0.3s;
  }
}

/* Ноутбуки (1024px–1919px) */
@media (min-width: 1024px) and (max-width: 1919px) {
  .open-button {
    padding: 14px 28px;
    font-size: 18px;
  }

  .modal-overlay {
    top: -15vh;
    left: -20vw;
    width: 120vw;
    height: 110vh;
  }
  .modal {
    background: #1e1e1e;
    padding: 30px;
    border-radius: 16px;
    width: 42.97vw;
    height: 720px;
    color: white;
    display: flex;
    flex-direction: column;
    gap: 14px;
    box-shadow: 0 12px 30px rgba(0, 0, 0, 0.6);
  }

  .modal h2 {
    margin-bottom: 10px;
    font-size: 32px;
    color: white;
  }

  .modal input {
    padding: 25px;
    border: none;
    border-radius: 8px;
    background: #2c2c2c;
    color: white;
    font-size: 18px;
  }

  .modal select {
    background: #2c2c2c url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' fill='white' viewBox='0 0 20 20'%3E%3Cpath d='M5.23 7.21a.75.75 0 011.06.02L10 10.94l3.71-3.71a.75.75 0 111.06 1.06l-4.24 4.24a.75.75 0 01-1.06 0L5.21 8.29a.75.75 0 01.02-1.08z'/%3E%3C/svg%3E") no-repeat right 1.5rem center / 1.5em !important;
    padding: 25px 2.5rem 25px 25px;
    border-radius: 8px;
    font-size: 18px;
  }

  .modal-actions button {
    font-size: 19px;
    position: relative;
    top: 20%;
    padding: 20px 26px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    flex: 1;
    transition: background-color 0.3s;
  }
}

/* Планшеты (768px–1023px) */
@media (max-width: 1023px) {
  .open-button {
    padding: 12px 24px;
    font-size: 17px;
  }

  .modal-overlay {
    top: -27vh;
    left: -17vw;
    width: 120vw;
    height: 110vh;
  }

  .modal h2 {
    font-size: 24px;
  }

  .modal input {
    padding: 13px;
    border: none;
    border-radius: 8px;
    background: #2c2c2c;
    color: white;
    font-size: 16px;
  }

  .modal select {
    background: #2c2c2c url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' fill='white' viewBox='0 0 20 20'%3E%3Cpath d='M5.23 7.21a.75.75 0 011.06.02L10 10.94l3.71-3.71a.75.75 0 111.06 1.06l-4.24 4.24a.75.75 0 01-1.06 0L5.21 8.29a.75.75 0 01.02-1.08z'/%3E%3C/svg%3E") no-repeat right 1rem center / 1em !important;
    appearance: none;
    -webkit-appearance: none;
    -moz-appearance: none;
    padding: 13px 2.5rem 13px 13px;
    border: none;
    border-radius: 8px;
    font-size: 16px;
  }

  .modal-actions button {
    font-size: 17px;
    position: relative;
    top: 20%;
    padding: 15px 26px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    flex: 1;
    transition: background-color 0.3s;
  }
}

/* Мобильные (481px–767px) */
@media (max-width: 767px) {
  .open-button {
    padding: 10px 22px;
    font-size: 16px;
  }

  .modal-overlay {
    top: -32vh;
    left: -15vw;
    width: 120vw;
    height: 110vh;
  }

  .modal h2 {
    font-size: 20px;
  }

  .modal input {
    padding: 13px;
    border: none;
    border-radius: 8px;
    background: #2c2c2c;
    color: white;
    font-size: 16px;
  }

  .modal select {
    background: #2c2c2c url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' fill='white' viewBox='0 0 20 20'%3E%3Cpath d='M5.23 7.21a.75.75 0 011.06.02L10 10.94l3.71-3.71a.75.75 0 111.06 1.06l-4.24 4.24a.75.75 0 01-1.06 0L5.21 8.29a.75.75 0 01.02-1.08z'/%3E%3C/svg%3E") no-repeat right 1rem center / 1em !important;
    appearance: none;
    -webkit-appearance: none;
    -moz-appearance: none;
    padding: 13px 2.5rem 13px 13px;
    border: none;
    border-radius: 8px;
    font-size: 16px;
  }

  .modal-actions button {
    font-size: 18px;
    position: relative;
    top: 20%;
    padding: 13px 24px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    flex: 1;
    transition: background-color 0.3s;
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

  .modal-overlay {
    top: -32vh;
    left: -13vw;
    width: 120vw;
    height: 110vh;
  }
}
</style>

<style>
/* Глобальные стили для Datepicker (без scoped) */
.custom-datepicker {
  --dp-input-padding: 20px;
  --dp-background-color: #2c2c2c;
  --dp-text-color: #fff;
  --dp-hover-color: #3a3a3a;
  --dp-hover-text-color: #fff;
  --dp-primary-color: #4caf50;
  --dp-primary-text-color: #fff;
  --dp-secondary-color: #666;
  --dp-border-color: none;
  --dp-menu-border-color: #444;
  --dp-border-radius: 8px;
  --dp-font-size: 19px;
  --dp-preview-font-size: 25px;
}

.dp__menu {
  border: 1px solid var(--dp-border-color);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
}

/* Стили для кнопок в Datepicker */
.dp__action_button {
  padding: 20px 100px;
  border-radius: 6px;
  font-weight: 500;
  transition: all 0.2s ease;
}

.dp__action_select {
  background: var(--dp-primary-color);
  color: var(--dp-primary-text-color);
}

.dp__action_select:hover {
  background: #3d8b40; /* Темнее на hover */
}

.dp__action_cancel {
  background: var(--dp-secondary-color);
  color: var(--dp-text-color);
}

.dp__action_cancel:hover {
  background: #555; /* Темнее на hover */
}

/* Скрытие кнопок, если не нужны */
.dp__action_buttons {
  display: flex;
  align-content: center;
  justify-content: space-between;
  gap: 10px;
  margin-inline-start: 0;
}

@media (max-width: 1023px) {
  .custom-datepicker {
    --dp-input-padding: 10px;
    --dp-border-radius: 8px;
    --dp-font-size: 17px;
    --dp-preview-font-size: 16px;
  }

  /* Стили для кнопок в Datepicker */
  .dp__action_button {
    padding: 15px 70px;
  }

  .dp__action_row {
    display: block;
  }
}

@media (max-width: 767px) {
  .custom-datepicker {
    --dp-input-padding: 10px;
    --dp-font-size: 17px;
    --dp-preview-font-size: 12px;
  }

  /* Стили для кнопок в Datepicker */
  .dp__action_button {
    padding: 15px 60px;
  }

  .dp__cell_inner {
    padding: 0;
    font-size: 14px; /* Уменьшаем размер шрифта для маленьких экранов */
  }

  .dp__menu_inner {
    padding: 2px 2px 0 2px;
  }

  .dp__action_row {
    padding-top: 0;
  }
}


.dp__action_cancel {
  position: relative;
  font-size: 0 !important;
}
.dp__action_cancel::after {
  content: 'Отмена' !important;
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
}

.dp__action_select {
  position: relative;
  font-size: 0 !important;
}
.dp__action_select::after {
  content: 'Выбрать' !important;
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
}
</style>
