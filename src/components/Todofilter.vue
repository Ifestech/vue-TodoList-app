<script setup>
import { defineProps, defineEmits } from 'vue'

// Props from parent
const props = defineProps({
  filter: {
    type: String,
    default: 'all'
  }
})

// Emits to parent
const emit = defineEmits(['change-filter'])
</script>

<template>
    <div class="flex justify-center gap-3">
        <!-- All -->
        <button class="px-3 py-1 rounded border" :class="filter === 'all'  ? 'bg-blue-500 text-white border-blue-500' 
        : 'bg-gray-200 dark:bg-gray-700 text-gray-900 dark:text-gray-100'"  @click="emit('change-filter', 'all')">
        All
        </button>

        <!-- Active -->
        <button class="px-3 py-1 rounded border" :class="filter === 'active' ? 'bg-blue-500 text-white border-blue-500' 
        : 'bg-gray-200 dark:bg-gray-700 text-gray-900 dark:text-gray-100'" @click="emit('change-filter', 'active')">
        Active
        </button>

        <!-- Completed -->
        <button class="px-3 py-1 rounded border" :class="filter === 'completed'  ? 'bg-blue-500 text-white border-blue-500' 
        : 'bg-gray-200 dark:bg-gray-700 text-gray-900 dark:text-gray-100'" @click="emit('change-filter', 'completed')">
        Completed
        </button>
    </div>
</template>
