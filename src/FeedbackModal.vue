<template>
  <div v-if="isOpen" class="overlay" @click.self="emit('close')">
    <div class="modal">
      <div class="modal-top">
        <h3>Оценка мероприятия</h3>
        <button class="close" @click="emit('close')">×</button>
      </div>

      <form @submit.prevent="sendForm">
        <label class="label">Оценка мероприятия *</label>
        <div class="stars">
          <button
            v-for="n in 5"
            :key="n"
            type="button"
            class="star"
            :class="{ active: n <= rating }"
            @click="setRating(n)"
          >
            ★
          </button>
        </div>
        <p v-if="error" class="error">{{ error }}</p>

        <label class="label">Комментарий</label>
        <textarea
          v-model="comment"
          rows="4"
          placeholder="Напишите ваш отзыв..."
        ></textarea>
        <div class="counter">{{ comment.length }} / 500</div>

        <button type="submit" class="submit-btn">Отправить отзыв</button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps(['isOpen'])
const emit = defineEmits(['close', 'submit'])

const rating = ref(0)
const comment = ref('')
const error = ref('')

function setRating(n) {
  rating.value = n
  error.value = ''
}

function sendForm() {
  if (rating.value === 0) {
    error.value = 'Выберите оценку от 1 до 5'
    return
  }
  if (comment.value.length > 500) {
    error.value = 'Комментарий не должен превышать 500 символов'
    return
  }

  // Сохранение в локальное хранилище (имитация БД)
  const list = JSON.parse(localStorage.getItem('reviews') || '[]')
  list.push({ rating: rating.value, comment: comment.value })
  localStorage.setItem('reviews', JSON.stringify(list))

  rating.value = 0
  comment.value = ''
  error.value = ''
  emit('submit')
}

function onEsc(e) {
  if (e.key === 'Escape' && props.isOpen) {
    emit('close')
  }
}

onMounted(() => window.addEventListener('keydown', onEsc))
onUnmounted(() => window.removeEventListener('keydown', onEsc))
</script>

<style scoped>
.overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 16px;
}

.modal {
  background: white;
  width: 100%;
  max-width: 420px;
  max-height: 90vh;
  overflow-y: auto;
  padding: 20px;
  border-radius: 10px;
}

.modal-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.close {
  background: none;
  border: none;
  font-size: 26px;
  cursor: pointer;
}

.label {
  display: block;
  margin-top: 14px;
  margin-bottom: 6px;
  font-weight: bold;
}

.stars {
  display: flex;
  gap: 8px;
}

.star {
  font-size: 28px;
  background: #f3f4f6;
  border: 1px solid #ccc;
  border-radius: 6px;
  width: 44px;
  height: 44px;
  color: #bbb;
  cursor: pointer;
}

.star.active {
  color: #f59e0b;
  border-color: #f59e0b;
}

textarea {
  width: 100%;
  box-sizing: border-box;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-family: inherit;
  font-size: 14px;
}

.counter {
  text-align: right;
  font-size: 12px;
  color: #666;
  margin-top: 4px;
}

.error {
  color: red;
  font-size: 13px;
  margin-top: 6px;
}

.submit-btn {
  margin-top: 16px;
  width: 100%;
  background: #2563eb;
  color: white;
  border: none;
  padding: 12px;
  border-radius: 6px;
  font-size: 15px;
  cursor: pointer;
}

/* Адаптация под телефон 390x844 */
@media (max-width: 480px) {
  .modal {
    max-width: 100%;
    height: 85vh;
  }

  .star {
    flex: 1;
    height: 52px;
    font-size: 32px;
  }

  textarea {
    font-size: 16px;
  }

  .submit-btn {
    padding: 15px;
    font-size: 16px;
  }
}
</style>
