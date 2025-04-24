<template>
    <div class="bg-white p-6 rounded-2xl shadow-xl text-center max-w-xs mx-auto">
      <h1 class="text-2xl font-semibold mb-4">Pomodoro Timer</h1>
      <div class="text-xl font-bold">
        {{ mode === 'focus' ? 'Focus Time' : 'Break Time' }}
      </div>
  
      <div class="text-5xl font-mono mb-6">{{ formattedTime }}</div>
      <div class="flex justify-center gap-4 flex-wrap">
        <button
          @click="toggleTimer"
          class="bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-2xl transition"
        >
          <i class="pi pi-pause" v-if="isActive"></i>
          <i class="pi pi-play" v-else></i>
        </button>
        <button
          @click="resetTimer"
          class="bg-gray-300 hover:bg-gray-400 text-black px-4 py-2 rounded-2xl transition"
        >
          <i class="pi pi-replay"></i>
        </button>
        <button
          @click="nextPomodoro"
          class="bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded-2xl transition"
        >
          <i class="pi pi-step-forward-alt"></i>
        </button>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed, onBeforeUnmount, defineEmits } from 'vue'
  
  const emit = defineEmits(['pomodoro-complete'])
  
  const mode = ref('focus') // 'focus' or 'break'
  const duration = ref(25 * 60) // in seconds, default to focus
  const timeLeft = ref(duration.value)
  const initialTime = 25 * 60
  const isActive = ref(false)
  const timer = ref(null) // Use a ref for the timer
  
  const formattedTime = computed(() => {
    const minutes = Math.floor(timeLeft.value / 60)
    const seconds = timeLeft.value % 60
    return `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`
  })
  
  const startTimer = () => {
    timer.value = setInterval(() => {
      timeLeft.value--
      if (timeLeft.value <= 0) {
        clearInterval(timer.value)
        if (mode.value === 'focus') {
          emit('pomodoro-complete') // Only trigger during focus
          mode.value = 'break'
          duration.value = 5 * 60
        } else {
          mode.value = 'focus'
          duration.value = 25 * 60
        }
        timeLeft.value = duration.value
        startTimer() // auto-start the next session
      }
    }, 1000)
  }
  
  const toggleTimer = () => {
    isActive.value = !isActive.value
    if (isActive.value) {
      startTimer()
    } else {
      clearInterval(timer.value) // Only clear if the timer exists
      timer.value = null
    }
  }
  
  const resetTimer = () => {
    clearInterval(timer.value)
    timer.value = null
    isActive.value = false
    timeLeft.value = initialTime
  }
  
  const nextPomodoro = () => {
  clearInterval(timer.value)
  timer.value = null
  isActive.value = false

  if (mode.value === 'focus') {
    emit('pomodoro-complete') // Only trigger during focus
    mode.value = 'break'
    duration.value = 5 * 60
    timeLeft.value = duration.value // Set timeLeft to 5 minutes for break
  } else {
    mode.value = 'focus'
    duration.value = 25 * 60
    timeLeft.value = duration.value // Reset timeLeft to 25 minutes for focus
  }
}

  
  onBeforeUnmount(() => {
    if (timer.value) {
      clearInterval(timer.value)
    }
  })
  </script>
  
  <style scoped>
  /* Add your custom styles here */
  </style>
  