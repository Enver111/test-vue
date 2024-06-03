<template>
  <div id="app">
    <Filters @onFilter="applyFilters" />
    <Loading v-if="isLoading" />
    <div class="characters">
      <CharacterCard v-for="character in characters" :key="character.id" :character="character" />
    </div>
    <Pagination :page="page" @prev="prevPage" @next="nextPage" />
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import CharacterCard from './views/CharacterCard.vue'
import Pagination from './views/Pagination.vue'
import Filters from './views/Filters.vue'
import Loading from './views/Loading.vue'

const characters = ref([])
const page = ref(1)
const isLoading = ref(false)
const nameFilter = ref('')
const statusFilter = ref('')
const fetchCharacters = async () => {
  isLoading.value = true
  let url = `https://rickandmortyapi.com/api/character/?page=${page.value}`
  if (nameFilter.value) url += `&name=${nameFilter.value}`
  if (statusFilter.value) url += `&status=${statusFilter.value}`
  const response = await fetch(url)
  const data = await response.json()
  characters.value = data.results
  isLoading.value = false
}
const applyFilters = (filters) => {
  nameFilter.value = filters.name
  statusFilter.value = filters.status
  page.value = 1
  fetchCharacters()
}
const prevPage = () => {
  if (page.value > 1) {
    page.value--
    fetchCharacters()
  }
}
const nextPage = () => {
  page.value++
  fetchCharacters()
}
onMounted(() => {
  fetchCharacters()
})
</script>
<style scoped>
.characters {
  display: flex;
  -webkit-box-pack: center;
  justify-content: center;
  -webkit-box-align: center;
  align-items: center;
  flex-wrap: wrap;
  max-width: 1920px;
  margin: auto;
}
</style>
