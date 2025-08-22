<script setup>
import { ref, computed } from 'vue'
import AddTask from './components/AddTask.vue'
import TodoList from './components/TodoList.vue'
import TodoFilter from './components/Todofilter.vue'

const theme = ref('light')
const todos = ref([])

// filter state
const filter = ref('all')

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

// Change filter
const setFilter = (newFilter) => {
  filter.value = newFilter
}

// Computed filtered todos
const filteredTodos = computed(() => {
  if (filter.value === 'active') {
    return todos.value.filter(todo => !todo.completed)
  }
  if (filter.value === 'completed') {
    return todos.value.filter(todo => todo.completed)
  }
  return todos.value
})
</script>

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

      <!-- Filter -->
      <TodoFilter :filter="filter" @change-filter="setFilter" />

      <!-- Task List Component -->
      <TodoList :todos="filteredTodos" @toggle-complete="toggleComplete" @delete-task="deleteTask" />
    </main>
  </div>
</template>
