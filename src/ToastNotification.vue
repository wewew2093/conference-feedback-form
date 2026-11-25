<template>
  <div v-if="show" class="toast">
    {{ text }}
  </div>
</template>

<script setup>
defineProps(['show', 'text'])
</script>

<style scoped>
.toast {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background: #16a34a;
  color: white;
  padding: 14px 20px;
  border-radius: 8px;
  font-weight: bold;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
}
</style>
