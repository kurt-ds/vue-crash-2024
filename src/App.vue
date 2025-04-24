<script setup>
import { ref } from 'vue'
import PomodoroTimer from './components/PomodoroTimer.vue'
import HeaderSection from './components/HeaderSection.vue'
import AddTaskForm from './components/AddTaskForm.vue'
import TaskList from './components/TaskList.vue'

const tasks = ref([])
const isAddTaskFormOpen = ref(false)


const toggleAddTaskForm = () => {
  isAddTaskFormOpen.value = !isAddTaskFormOpen.value
}

const addTask = (newTaskName, newEstPomodoros) => {
  const newTask = {
    name: newTaskName,
    estPomodoros: newEstPomodoros,
    currentPomodoros: 0,
    isEditing: false,
    isActive: tasks.value.length === 0,
  }

  tasks.value.push(newTask)
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
  const task = tasks.value[payload.index]
  task.name = payload.newName
  task.estPomodoros = payload.newEstPomodoros
  task.currentPomodoros = payload.newCurrentPomodoros
  task.isEditing = false
}


const deleteTask = (index) => {
  if (confirm('Are you sure you want to delete this task?')) {
    tasks.value.splice(index, 1)
  }
}

const handlePomodoroComplete = () => {
  const index = tasks.value.findIndex(task => task.isActive)
if (index !== -1) {
  const updated = { ...tasks.value[index] }
  updated.currentPomodoros++
  tasks.value[index] = updated
}
}

const setActiveTask = (index) => {
  tasks.value.forEach((task, i) => {
    task.isActive = i === index; // Set the clicked task as active, others as inactive
  });
}

</script>

<template>
  <div class="flex flex-col gap-5">
  <PomodoroTimer  @pomodoro-complete="handlePomodoroComplete"/>
  <div class="flex flex-col gap-2 w-full min-h-50 p-3 shadow-2xl rounded-2xl bg-cream">

    <HeaderSection @toggle-add-task-form="toggleAddTaskForm" />

    <AddTaskForm v-if="isAddTaskFormOpen" @add-task="addTask" @close-form="closeAddTaskForm" />

    <TaskList
      :tasks="tasks"
      @start-editing="startEditing"
      @cancel-editing="cancelEditing"
      @save-editing="saveEditing"
      @delete-task="deleteTask"
      @set-active-task="setActiveTask"
    />
  </div>
</div>
</template>

<style scoped>
/* You can add global styles here if needed, or keep component-specific styles in their respective <style scoped> blocks */
</style>
