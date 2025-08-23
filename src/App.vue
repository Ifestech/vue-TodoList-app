<script setup>
import { ref, computed } from 'vue'
import AddTask from './components/AddTask.vue'
import TodoList from './components/TodoList.vue'
import TodoFilter from './components/Todofilter.vue'

const theme = ref('light')
const todos = ref([])
const history = ref([]) // ✅ Completed tasks history
const showHistory = ref(false) // ✅ Toggle state for history

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

  // ✅ Move to history if completed
  if (isCompleted) {
    history.value.push({ ...todos.value[index], completedAt: new Date() })
  }
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

const editTask = (index, newText) => {
  todos.value[index].text = newText
}
</script>

<template>
  <div :class="theme" class="dark:bg-gray-800 dark:text-amber-50 bg-gray-200 ">
    <!-- Header -->
    <header class="flex justify-between items-center p-4 ">
      <h1 class="text-2xl font-bold text-gray-900 dark:text-gray-100">Todo App</h1>
      <button @click="toggleTheme" class="px-3 py-1 rounded bg-gray-300 dark:bg-gray-700 text-gray-900 dark:text-gray-100" >
        {{ theme === 'light' ? '🌙 Dark' : '☀️ Light' }}
      </button>
    </header>

    <!-- Main -->
    <main class="p-4 max-w-xl mx-auto space-y-6">
      <!-- Add Task Input -->
      <AddTask @add-task="addTask" />

      <!-- Filter -->
      <TodoFilter :filter="filter" @change-filter="setFilter" />

      <!-- Task List Component -->
      <TodoList  :todos="filteredTodos" @toggle-complete="toggleComplete" @delete-task="deleteTask" @edit-task="editTask" />

      <!-- ✅ Completed Tasks History with Toggle -->
      <section v-if="history.length" class="mt-6 p-4 bg-gray-100 dark:bg-gray-700 rounded-lg shadow">
        <button @click="showHistory = !showHistory" class="w-full px-3 py-2 mb-2 rounded bg-blue-500 text-white font-medium" >
          {{ showHistory ? 'Hide Completed Tasks 📂' : 'Show Completed Tasks 📂' }}
        </button>

        <transition name="fade">
          <div v-if="showHistory">
            <h2 class="text-lg font-semibold mb-2">📜 Completed Task History</h2>
            <ul class="space-y-2">
              <li v-for="(task, i) in history" :key="i" class="text-sm text-gray-700 dark:text-gray-300 flex justify-between">
                <span>{{ task.text }}</span>
                <span class="text-xs text-gray-500">
                  ({{ new Date(task.completedAt).toLocaleTimeString() }})
                </span>
              </li>
            </ul>
          </div>
        </transition>
      </section>
    </main>
  </div>
</template>

<style>
/* Simple fade animation */
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.20s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
</style>
