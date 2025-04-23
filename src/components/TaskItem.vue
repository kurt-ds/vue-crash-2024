<script setup>
import { ref, defineProps, defineEmits, onMounted, watch } from 'vue'

const props = defineProps({
  task: {
    type: Object,
    required: true,
  },
  index: {
    type: Number,
    required: true,
  },
})

const emit = defineEmits(['start-editing', 'cancel-editing', 'save-editing', 'delete-task'])

const isEditing = ref(props.task.isEditing)
const taskName = ref(props.task.name)
const estPomodoros = ref(props.task.estPomodoros)
const currentPomodoros = ref(props.task.currentPomodoros)
const originalTask = ref({ ...props.task })
const currentPomodorosError = ref('')

const startEdit = () => {
  originalTask.value = { ...props.task }
  isEditing.value = true
  emit('start-editing', props.task)
}

const cancelEdit = () => {
  taskName.value = originalTask.value.name
  estPomodoros.value = originalTask.value.estPomodoros
  currentPomodoros.value = originalTask.value.currentPomodoros
  isEditing.value = false
  currentPomodorosError.value = '' // Clear any error on cancel
  emit('cancel-editing', props.task)
}

const saveEdit = () => {
  if (parseInt(currentPomodoros.value) > parseInt(estPomodoros.value)) {
    currentPomodorosError.value = 'Current cannot be greater than Estimated.'
    return
  }
  emit('save-editing', {
    index: props.index,
    newName: taskName.value,
    newEstPomodoros: parseInt(estPomodoros.value),
    newCurrentPomodoros: parseInt(currentPomodoros.value),
  })
  isEditing.value = false
  currentPomodorosError.value = '' // Clear error on successful save
}

const deleteTask = () => {
  if (confirm('Are you sure you want to delete this task?')) {
    emit('delete-task', props.index)
  }
}

onMounted(() => {
  isEditing.value = props.task.isEditing
  taskName.value = props.task.name
  estPomodoros.value = props.task.estPomodoros
  currentPomodoros.value = props.task.currentPomodoros
})

// Optional: Add a watch to provide real-time error feedback while editing
watch(currentPomodoros, (newValue) => {
  if (isEditing.value && parseInt(newValue) > parseInt(estPomodoros.value)) {
    currentPomodorosError.value = 'Current cannot be greater than Estimated.'
  } else {
    currentPomodorosError.value = ''
  }
})

watch(estPomodoros, (newValue) => {
  // Also re-validate if estimated pomodoros are reduced below current
  if (isEditing.value && parseInt(currentPomodoros.value) > parseInt(newValue)) {
    currentPomodorosError.value = 'Current cannot be greater than Estimated.'
  } else if (!isEditing.value) {
    // Clear error if not editing and values are valid
    currentPomodorosError.value = ''
  }
})
</script>

<template>
  <li class="flex justify-between items-center bg-sage px-3 py-1 rounded">
    <div v-if="!isEditing">
      <div>{{ taskName }}</div>
    </div>

    <div v-else class="flex items-center gap-2 w-full">
      <div class="flex flex-col gap-1 w-full">
        <input
          v-model="taskName"
          class="border border-gray-400 rounded px-2 py-1 w-full"
          placeholder="Task Name"
        />
        <div class="flex gap-2">
          <input
            type="number"
            v-model="currentPomodoros"
            class="border border-gray-400 rounded px-2 py-1 w-1/2"
            placeholder="Done Pomodoros"
            min="0"
          />
          <span class="text-3xl"> / </span>
          <input
            type="number"
            v-model="estPomodoros"
            class="border border-gray-400 rounded px-2 py-1 w-1/2"
            placeholder="Est. Pomodoros"
            min="0"
          />
        </div>
        <div v-if="currentPomodorosError" class="text-red-500 text-sm">
          {{ currentPomodorosError }}
        </div>
      </div>
      <button @click="saveEdit" class="text-green-600">
        <i class="pi pi-save"></i>
      </button>
      <button @click="cancelEdit" class="text-gray-600">
        <i class="pi pi-times"></i>
      </button>
    </div>

    <div v-if="!isEditing" class="flex gap-2 ml-4 items-center">
      <div class="text-sm text-gray-600">{{ currentPomodoros }} / {{ estPomodoros }}</div>
      <button @click="startEdit" class="text-khaki">
        <i class="pi pi-pencil"></i>
      </button>
      <button @click="deleteTask" class="text-red-800">
        <i class="pi pi-trash"></i>
      </button>
    </div>
  </li>
</template>

<style scoped>
/* You can add component-specific styles here */
</style>
