<template>
  <div :class="theme">
    <header class="flex justify-between items-center p-4 bg-gray-200 dark:bg-gray-800">
      <h1 class="text-2xl font-bold text-gray-900 dark:text-gray-100">Todo App</h1>
      <button @click="toggleTheme" class="px-3 py-1 rounded bg-gray-300 dark:bg-gray-700 text-gray-900 dark:text-gray-100" >
        {{ theme === 'light' ? '🌙 Dark' : '☀️ Light' }}
      </button>
    </header>
    <main class="p-4">
      <!-- Components will go here -->
       <AddTask />
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import AddTask from './components/AddTask.vue'

const theme = ref('light')

const toggleTheme = () => {
  theme.value = theme.value === 'light' ? 'dark' : 'light'
  document.documentElement.classList.toggle('dark', theme.value === 'dark')
}
</script>