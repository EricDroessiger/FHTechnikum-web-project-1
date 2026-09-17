//Liste rendern
<script setup lang="ts">
import type {Todo} from "../models/todo.ts";

interface Props{
  todos: Todo[]
}

const props = defineProps<Props>()
</script>

<template>
  <ul>
    <li v-for="todo in props.todos" :key="todo.id">
      {{ todo.text }} — {{ todo.done ? 'erledigt' : 'offen' }}
    </li>
  </ul>
</template>
