<template>
  <div :class="theme">
    <header class="flex justify-between items-center p-4 bg-gray-200 dark:bg-gray-800">
      <h1 class="text-2xl font-bold text-gray-900 dark:text-gray-100">Todo App</h1>
      <button @click="toggleTheme" class="px-3 py-1 rounded bg-gray-300 dark:bg-gray-700 text-gray-900 dark:text-gray-100" >
        {{ theme === 'light' ? '🌙 Dark' : '☀️ Light' }}
      </button>
    </header>
    <main class="p-4 max-w-xl mx-auto space-y-4">
      <!-- Components will go here -->
      <AddTask @add-task="AddTask" />
       
            <!-- Task List -->
      <div v-if="todos.length" class="space-y-2">

        <TodoItem v-for="(todo, index) in todos"
        :key="index"
        :todo="todo.text"
        :completed="todo.completed"
        @toggle-complete="toggleComplete(index, $event)"
        @delete-task="deleteTask(index)" />
      </div>
      <p v-else class="text-gray-500 dark:text-gray-400">No tasks yet. Add one above 👆</p>
    </main>
  </div>
</template>


<script setup>
import { ref } from 'vue'
import AddTask from './components/AddTask.vue'
import TodoItem from './components/TodoItem.vue'

const theme = ref('light')
const todos = ref([])

const toggleTheme = () => {
  theme.value = theme.value === 'light' ? 'dark' : 'light'
  document.documentElement.classList.toggle('dark', theme.value === 'dark')
}

const addTask = (task) => {
  todos.value.push({ text: task, completed: false })
}

const toggleComplete = (index, isCompleted) => {
  todos.value[index].completed = isCompleted
}

const deleteTask = (index) => {
  todos.value.splice(index, 1)
}
</script>