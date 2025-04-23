<script setup>
import { ref } from 'vue'
import HeaderSection from './components/HeaderSection.vue'
import AddTaskForm from './components/AddTaskForm.vue'
import TaskList from './components/TaskList.vue'

const tasks = ref([])
const isAddTaskFormOpen = ref(false)

const toggleAddTaskForm = () => {
  isAddTaskFormOpen.value = !isAddTaskFormOpen.value
}

const addTask = (newTaskName, newEstPomodoros) => {
  tasks.value.push({
    name: newTaskName,
    estPomodoros: newEstPomodoros,
    currentPomodoros: 0,
    isEditing: false,
    isActive: tasks.value.length === 0,
  })
}

const closeAddTaskForm = () => {
  isAddTaskFormOpen.value = false
}

const startEditing = (task) => {
  task.originalName = task.name
  task.isEditing = true
}

const cancelEditing = (task) => {
  task.name = task.originalName
  task.isEditing = false
}

const saveEditing = (payload) => {
  tasks.value[payload.index].name = payload.newName
  tasks.value[payload.index].isEditing = false
}

const deleteTask = (index) => {
  if (confirm('Are you sure you want to delete this task?')) {
    tasks.value.splice(index, 1)
  }
}
</script>

<template>
  <div class="flex flex-col gap-2">
    <HeaderSection @toggle-add-task-form="toggleAddTaskForm" />

    <AddTaskForm v-if="isAddTaskFormOpen" @add-task="addTask" @close-form="closeAddTaskForm" />

    <TaskList
      :tasks="tasks"
      @start-editing="startEditing"
      @cancel-editing="cancelEditing"
      @save-editing="saveEditing"
      @delete-task="deleteTask"
    />
  </div>
</template>

<style scoped>
/* You can add global styles here if needed, or keep component-specific styles in their respective <style scoped> blocks */
</style>
