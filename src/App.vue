<template>
  <div :class="theme">
    <!-- Header -->
    <header class="flex justify-between items-center p-4 bg-gray-200 dark:bg-gray-800">
      <h1 class="text-2xl font-bold text-gray-900 dark:text-gray-100">Todo App</h1>
      <button
        @click="toggleTheme"
        class="px-3 py-1 rounded bg-gray-300 dark:bg-gray-700 text-gray-900 dark:text-gray-100"
      >
        {{ theme === 'light' ? '🌙 Dark' : '☀️ Light' }}
      </button>
    </header>

    <!-- Main -->
    <main class="p-4 max-w-xl mx-auto space-y-4">
      <!-- Add Task Input -->
      <AddTask @add-task="addTask" />

      <!-- Task List Component -->
      <TodoList
        :todos="todos"
        @toggle-complete="toggleComplete"
        @delete-task="deleteTask"
      />
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import AddTask from './components/AddTask.vue'
import TodoList from './components/TodoList.vue'

const theme = ref('light')
const todos = ref([])

// Theme toggle
const toggleTheme = () => {
  theme.value = theme.value === 'light' ? 'dark' : 'light'
  document.documentElement.classList.toggle('dark', theme.value === 'dark')
}

// Add new task
const addTask = (task) => {
  todos.value.push({ text: task, completed: false })
}

// Mark task complete/incomplete
const toggleComplete = (index, isCompleted) => {
  todos.value[index].completed = isCompleted
}

// Delete task
const deleteTask = (index) => {
  todos.value.splice(index, 1)
}
</script>

