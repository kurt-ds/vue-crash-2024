<script setup>
import { defineEmits } from 'vue'

const emit = defineEmits(['clear-tasks'])

const handleClear = () => {
  emit('clear-tasks')
}
</script>

<template>
  <button
    @click="handleClear"
    class="bg-khaki hover:bg-haze p-2 shadow-xl rounded-md text-cream max-w-min self-center"
  >
    Clear
  </button>
</template>
