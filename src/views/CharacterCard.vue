<template>
  <div class="character-cards">
    <div class="character-image">
      <img :src="character?.image" :alt="`Image of ${character?.name}`" />
    </div>
    <div class="character-card">
      <div class="section">
        <a href="#"
          ><h2>{{ character?.name }}</h2></a
        >
        <div class="status-indicator">
          <div class="status">
            <span
              class="status-circle"
              :class="{
                alive: character?.status === 'Alive',
                dead: character?.status === 'Dead',
                unknown: character?.status === 'unknown'
              }"
            ></span>
            {{ character?.status + ' - ' + character?.species }}
          </div>
        </div>
      </div>
      <div class="section">
        <span>Last known location:</span>
        <a href="#">{{ character?.location.name }}</a>
      </div>
      <div class="section">
        <span>First seen in: </span>
        <a href="#">{{ episodeName }}</a>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, defineProps, onMounted } from 'vue'
const props = defineProps({
  character: Object
})
const episodeName = ref('Loading...')
onMounted(async () => {
  if (props.character?.episode.length > 0) {
    episodeName.value = await fetchEpisodeName(props.character?.episode[0])
  }
})
const fetchEpisodeName = async (episodeUrl: string) => {
  try {
    const response = await fetch(episodeUrl)
    const data = await response.json()
    return data.name
  } catch (error) {
    console.error('Ошибка при получении данных эпизода:', error)
  }
}
</script>

<style scoped>
.character-cards {
  width: 600px;
  height: 220px;
  display: flex;
  overflow: hidden;
  background: rgb(60, 62, 68);
  border-radius: 0.5rem;
  margin: 0.75rem;
  box-shadow:
    rgba(0, 0, 0, 0.1) 0px 4px 6px -1px,
    rgba(0, 0, 0, 0.06) 0px 2px 4px -1px;
}
.character-card {
  flex: 3 1 0%;
  position: relative;
  padding: 0.75rem;
  color: rgb(255, 255, 255);
  display: flex;
  flex-direction: column;
}
.character-image img {
  width: 100%;
  height: 100%;
  margin: 0px;
  opacity: 1;
  transition: opacity 0.5s ease 0s;
  object-position: center center;
  object-fit: cover;
}
.character-card a {
  text-decoration: none;
  color: white;
  cursor: pointer;
}
.character-card a:hover {
  color: rgb(255, 152, 0);
}
.character-card h2 {
  font-size: 1.5rem;
  margin: 0;
  padding: 0;
  font-weight: 800;
  line-height: 30px;
}
.status-circle {
  height: 0.5rem;
  width: 0.5rem;
  margin-right: 0.375rem;
  border-radius: 50%;
}
.alive {
  background-color: rgb(85, 204, 68);
}
.dead {
  background-color: rgb(214, 61, 46);
}
.unknown {
  background-color: rgb(214, 150, 46);
}
.section {
  flex: 1 1 0%;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.section:first-child {
  justify-content: flex-start;
}
.section:last-child {
  justify-content: flex-end;
}
.status {
  display: flex;
  align-items: center;
  text-transform: capitalize;
  font-size: 16px;
  font-weight: 500;
}
.section span {
  color: rgb(158, 158, 158);
  font-size: 16px;
  font-weight: 500;
}
.section a {
  font-size: 18px;
}
</style>
