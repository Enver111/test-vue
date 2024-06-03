<template>
  <div class="pages">
    <button @click="props.onPrev" :disabled="props.page <= 1">Назад</button>
    <button @click="props.onNext">Вперед</button>
  </div>
</template>

<script setup lang="ts">
import { defineProps } from 'vue'

const props = defineProps({
  page: Number,
  onPrev: Function,
  onNext: Function
})
</script>
<style scoped>
.pages {
  display: flex;
  justify-content: center;
  margin: 20px 0;
}
</style>
