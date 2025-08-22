<template>
  <div>
    <!-- If tasks exist -->
    <div v-if="todos.length" class="space-y-2">
      <TodoItem
        v-for="(todo, index) in todos"
        :key="index"
        :todo="todo.text"
        :completed="todo.completed"
        @toggle-complete="(isCompleted) => $emit('toggle-complete', index, isCompleted)"
        @delete-task="$emit('delete-task', index)"
      />
    </div>

    <!-- Empty state -->
    <p v-else class="text-gray-500 dark:text-gray-400">
      No tasks yet. Add one above 👆
    </p>
  </div>
</template>

<script setup>
import TodoItem from './TodoItem.vue'

defineProps({
  todos: {
    type: Array,
    required: true
  }
})

defineEmits(['toggle-complete', 'delete-task'])
</script>
