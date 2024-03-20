

<template>
  <div class="container">
    <the-calendar
      :initDate="initDate"
      :lang="initLang"
      @pickedDay="date => pickedDay(date)"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue'

import TheCalendar from './components/calendar/TheCalendar.vue'

// const initDate = ref('2024-3-21')
const initLang = ref('en')

const pickedDay = (date) => {
  console.log(`Выбранная дата: ${date}`)
}
</script>

<style scoped>
.container {
  max-width: 1199px;
  margin: 0 auto;
  padding: 15px;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}
</style>
