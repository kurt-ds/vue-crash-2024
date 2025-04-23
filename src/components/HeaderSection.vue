<script setup>
import { defineEmits } from 'vue'

const emit = defineEmits(['toggle-add-task-form'])

const emitToggleAddTaskForm = () => {
  emit('toggle-add-task-form')
}
</script>

<template>
  <div class="w-full flex justify-between relative">
    <h1 class="text-3xl font-bold">Todo List</h1>
    <button
      @click="emitToggleAddTaskForm"
      class="bg-khaki hover:bg-haze p-2 shadow-xl rounded-md text-cream"
    >
      Add Task
    </button>
  </div>
</template>

<style scoped>
/* You can add component-specific styles here */
</style>
