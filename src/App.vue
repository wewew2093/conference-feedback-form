<template>
  <div class="page">
    <header class="header">Конференции.РФ</header>

    <main class="card">
      <p class="status">Статус: Мероприятие завершено</p>
      <h2>Конференция «Цифровые технологии»</h2>
      <p>Заявка № 105 • Участник</p>

      <button v-if="!sent" class="btn" @click="isOpen = true">
        Оставить отзыв
      </button>

      <p v-else class="sent-text">Ваш отзыв отправлен</p>
    </main>

    <FeedbackModal
      :isOpen="isOpen"
      @close="isOpen = false"
      @submit="onSent"
    />

    <ToastNotification
      :show="showToast"
      text="Спасибо за отзыв!"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import FeedbackModal from './FeedbackModal.vue'
import ToastNotification from './ToastNotification.vue'

const isOpen = ref(false)
const sent = ref(false)
const showToast = ref(false)

function onSent() {
  isOpen.value = false
  sent.value = true
  showToast.value = true

  setTimeout(() => {
    showToast.value = false
  }, 3000)
}
</script>

<style>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #f2f4f7;
}

.header {
  background: #1d4ed8;
  color: white;
  padding: 16px 24px;
  font-size: 20px;
  font-weight: bold;
}

.card {
  max-width: 600px;
  margin: 40px auto;
  background: white;
  padding: 24px;
  border-radius: 10px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}

.status {
  color: #666;
  font-size: 14px;
}

.btn {
  margin-top: 16px;
  background: #2563eb;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 6px;
  font-size: 15px;
  cursor: pointer;
}

.sent-text {
  margin-top: 16px;
  color: #16a34a;
  font-weight: bold;
}
</style>
