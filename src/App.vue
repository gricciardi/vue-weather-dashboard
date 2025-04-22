<template>
  <div :class="['app', { dark: isDark }]">
    <button @click="isDark = !isDark" class="toggle">
      {{ isDark ? '☀️' : '🌙' }}
    </button>
    <h1>WeatherNow</h1>
    <WeatherCard :weather="weather" />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import WeatherCard from './components/WeatherCard.vue'

const isDark = ref(false)
const weather = ref(null)

onMounted(async () => {
  const res = await fetch(
    'https://api.open-meteo.com/v1/forecast?latitude=47.37&longitude=8.54&current_weather=true'
  )
  const data = await res.json()
  weather.value = data.current_weather
})
</script>

<style scoped>
.app {
  text-align: center;
  padding: 60px 20px;
  min-height: 100vh;
  transition: background 0.3s, color 0.3s;
}

.toggle {
  position: absolute;
  top: 20px;
  right: 20px;
  background: #007acc;
  color: white;
  border: none;
  border-radius: 50px;
  padding: 10px 16px;
  font-size: 1.2rem;
  cursor: pointer;
}

.dark {
  background: #1e1e2f;
  color: white;
}
</style>
