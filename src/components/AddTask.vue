<template>
  <form @submit.prevent="addTask" class="flex gap-2">
    <input
      v-model="task"
      type="text"
      placeholder="Enter a new task..."
      class="flex-1 px-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 dark:bg-gray-800 dark:text-gray-100"
    />
    <button
      type="submit"
      class="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition"
    >
      Add
    </button>
  </form>
</template>

<script setup>
import { ref } from 'vue'

const task = ref('')

// Emit event when new task is added
const emit = defineEmits(['add-task'])

const addTask = () => {
  if (task.value.trim() !== '') {
    emit('add-task', task.value.trim())
    task.value = '' // reset input
  }
}
</script>

