<script setup>
import { ref, defineEmits } from 'vue'

const newTask = ref('')
const newEstPomodoros = ref(1)
const emit = defineEmits(['add-task', 'close-form'])

const handleAddTask = () => {
  if (newTask.value.trim() !== '') {
    emit('add-task', newTask.value, newEstPomodoros.value)
    newTask.value = ''
    emit('close-form')
  }
}

const handleCancel = () => {
  emit('close-form')
}
</script>

<template>
  <div class="fixed inset-0 bg-khaki/50 flex justify-center items-center z-50">
    <form
      @submit.prevent="handleAddTask"
      class="bg-cream rounded-lg shadow-xl p-6 w-full max-w-md flex flex-col gap-1.5"
    >
      <h2 class="text-xl font-semibold mb-4">Add New Task</h2>
      <label for="newTask" class="block mb-2">Task Name:</label>
      <input
        type="text"
        id="newTask"
        v-model="newTask"
        class="border border-gray-300 rounded px-3 py-2 w-full mb-4"
        placeholder="Enter task..."
      />
      <label for="newEstPomdoros" class="block mb-2">Est Pomodoros:</label>
      <input
        type="number"
        id="newEstPomdoros"
        v-model="newEstPomodoros"
        class="border border-gray-300 rounded px-3 py-2 w-full mb-4"
        placeholder=""
      />
      <div class="flex justify-end gap-2">
        <button
          type="button"
          @click="handleCancel"
          class="bg-gray-300 hover:bg-gray-400 text-black px-4 py-2 rounded"
        >
          Cancel
        </button>
        <button
          type="submit"
          class="bg-khaki hover:bg-haze text-white px-4 py-2 rounded ease-linear hover:cursor-grab"
        >
          Add
        </button>
      </div>
    </form>
  </div>
</template>

<style scoped>
/* You can add component-specific styles here */
</style>
