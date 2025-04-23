<template>
  <div :class="['app', { dark: isDark }]">
    <button @click="isDark = !isDark" class="toggle">
      {{ isDark ? '☀️' : '🌙' }}
    </button>

    <h1>WeatherNow – {{ location }}</h1>

    <WeatherCard :weather="current" />

    <h2>Next 3 Days</h2>
    <div class="forecast-grid" v-if="forecast.length > 0">
      <div class="forecast" v-for="(day, i) in forecast.slice(1, 4)" :key="day.date">
        <p>{{ day.date }}</p>
        <p>🌡 {{ day.temp_min }}°C – {{ day.temp_max }}°C</p>
      </div>
    </div>
  </div>
</template>

import { ref, onMounted } from 'vue'
import WeatherCard from './components/WeatherCard.vue'

const isDark = ref(false)
const current = ref(null)
const forecast = ref([])
const location = ref('Zürich') // Default fallback

onMounted(async () => {
  const lat = 47.37
  const lon = 8.54

  // Wetterdaten holen
  const res = await fetch(
    `https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${lon}&current_weather=true&daily=temperature_2m_max,temperature_2m_min&timezone=auto`
  )
  const data = await res.json()
  current.value = data.current_weather
  forecast.value = data.daily.time.map((date, i) => ({
    date,
    temp_max: data.daily.temperature_2m_max[i],
    temp_min: data.daily.temperature_2m_min[i]
  }))

  // Ortsname holen
  const res2 = await fetch(
    `https://geocode.maps.co/reverse?lat=${lat}&lon=${lon}`
  )
  const loc = await res2.json()
  location.value = loc.address?.city || loc.address?.town || 'Unbekannt'
})


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
