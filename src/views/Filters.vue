<template>
  <div class="filtres">
    <input v-model="nameFilter" placeholder="Имя персонажа" />
    <select v-model="statusFilter">
      <option value="">All</option>
      <option value="alive">Alive</option>
      <option value="dead">Dead</option>
      <option value="unknown">Unknown</option>
    </select>
    <button @click="onFilter">Применить</button>
  </div>
</template>

<script setup lang="ts">
import { ref, defineEmits } from 'vue'
const emits = defineEmits(['onFilter'])
const nameFilter = ref('')
const statusFilter = ref('')
const onFilter = () => {
  emits('onFilter', { name: nameFilter.value, status: statusFilter.value })
}
</script>

<style scoped>
.filtres {
  display: flex;
  justify-content: center;
  margin: 20px 0;
}
.filtres select {
  cursor: pointer;
}
.filtres button {
  cursor: pointer;
}
</style>
