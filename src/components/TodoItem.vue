//einzelnes Todo mit Checkbox
<script setup lang="ts">
import type {Todo} from "../models/todo.ts";

interface Props{
  todo: Todo
}

const props = defineProps<Props>()

const emit = defineEmits<{
  (event: 'toggle', id: number): void
}>()

function toggleTodo() {
  emit('toggle', props.todo.id)
}
</script>

<template>
  <li>
    <label>
      <input
        type="checkbox"
        :checked="props.todo.done"
        @change="toggleTodo"
      />

      {{ props.todo.text }}
    </label>
  </li>
</template>
