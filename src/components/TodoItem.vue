<template>
    <div class="flex items-center justify-between p-2 rounded bg-gray-100 dark:bg-gray-700 text-gray-900 dark:text-gray-100">
        <!-- Task Text -->
        <div class="flex items-center gap-2">
            <input type="checkbox" v-model="localCompleted" @change="toggleComplete" />
            <span :class="{ 'line-through text-gray-400 dark:text-gray-500': localCompleted }"> {{ todo }}</span>
        </div>

        <!-- Delete Button -->
        <button @click="$emit('delete-task')" class="text-red-500 hover:text-red-700 transition"> ✖ </button>
    </div>
</template>

<script setup>                  
import { ref, watch } from 'vue'

const props = defineProps({
  todo: String,
  completed: Boolean
})

const emit = defineEmits(['toggle-complete', 'delete-task'])

const localCompleted = ref(props.completed)

// Watch local state to sync with parent
watch(localCompleted, (newVal) => {
  emit('toggle-complete', newVal)
})

const toggleComplete = () => {
  emit('toggle-complete', localCompleted.value)
}
</script>
