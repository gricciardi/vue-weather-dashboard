<template>
  <div class="card" v-if="weather">
    <h2>🌡 Temperature</h2>
    <p>{{ weather.temperature }}°C</p>
    <h3>💨 Wind</h3>
    <p>{{ weather.windspeed }} km/h</p>
  </div>
</template>

<script setup>
defineProps({
  weather: Object
})
</script>

<style scoped>
.card {
  background: white;
  padding: 30px;
  max-width: 360px;
  margin: 0 auto;
  border-radius: 14px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
  transition: background 0.3s, color 0.3s;
}

h2, h3 {
  margin-bottom: 8px;
}

.dark .card {
  background: #2f2f3f;
  color: white;
}
</style>
