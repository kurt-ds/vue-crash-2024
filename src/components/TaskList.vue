<script setup>
import { defineEmits } from 'vue'
import TaskItem from './TaskItem.vue'

const props = defineProps({
  tasks: {
    type: Array,
    required: true,
  },
})

const emit = defineEmits([
  'start-editing',
  'cancel-editing',
  'save-editing',
  'delete-task',
  'set-active-task' // 👈 Add this line
])


</script>

<template>
  <ul class="flex flex-col gap-1.5 mt-3 mx-8">
    <TaskItem
      v-for="(task, index) in tasks"
      :key="index"
      :task="task"
      :index="index"
      @start-editing="$emit('start-editing', $event)"
      @cancel-editing="$emit('cancel-editing', $event)"
      @save-editing="(payload) => $emit('save-editing', payload)"
      @delete-task="(index) => $emit('delete-task', index)"
      @click="$emit('set-active-task', index)"
    />
  </ul>
</template>

<style scoped>
/* You can add component-specific styles here */
</style>
