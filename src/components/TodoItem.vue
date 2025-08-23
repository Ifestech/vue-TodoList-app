<template>
  <div class="flex items-center justify-between bg-gray-100 dark:bg-gray-800 p-2 rounded-lg">
    <!-- Task display or edit input -->
    <div class="flex items-center gap-2">
      <!-- Checkbox -->
      <input
        type="checkbox"
        v-model="localCompleted"
        @change="$emit('toggle-complete', localCompleted)"
        class="cursor-pointer"
      />

      <!-- Normal text (when not editing) -->
      <span
        v-if="!isEditing"
        :class="[
          'cursor-pointer',
          localCompleted ? 'line-through text-gray-500' : 'text-gray-900 dark:text-white'
        ]"
      >
        {{ todo }}
      </span>

      <!-- Edit input (when editing) -->
      <input
        v-else
        type="text"
        v-model="editedText"
        class="px-2 py-1 rounded border focus:outline-none focus:ring"
        @keyup.enter="saveEdit"
        @blur="saveEdit"
      />
    </div>

    <!-- Buttons -->
    <div class="flex gap-2">
      <!-- Edit button (only if not completed) -->
      <button
        v-if="!isEditing"
        @click="startEditing"
        class="text-blue-500 hover:underline"
      >
        Edit
      </button>

      <!-- Save button (only in edit mode) -->
      <button
        v-if="isEditing"
        @click="saveEdit"
        class="text-green-500 hover:underline"
      >
        Save
      </button>

      <!-- Cancel button -->
      <button
        v-if="isEditing"
        @click="cancelEdit"
        class="text-gray-400 hover:underline"
      >
        Cancel
      </button>

      <!-- Delete button -->
      <button @click="$emit('delete-task')" class="text-red-500 hover:underline">
        Delete
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'

// Props
const props = defineProps({
  todo: { type: String, required: true },
  completed: { type: Boolean, required: true }
})

// Emits
const emit = defineEmits(['toggle-complete', 'delete-task', 'edit-task'])

// Local states
const localCompleted = ref(props.completed)
const isEditing = ref(false)
const editedText = ref(props.todo)

// Keep checkbox synced with parent
watch(() => props.completed, (val) => {
  localCompleted.value = val
})

// Start editing
const startEditing = () => {
  isEditing.value = true
  editedText.value = props.todo
}

// Save changes
const saveEdit = () => {
  if (editedText.value.trim()) {
    emit('edit-task', editedText.value.trim())
  }
  isEditing.value = false
}

// Cancel edit
const cancelEdit = () => {
  isEditing.value = false
  editedText.value = props.todo
}
</script>
