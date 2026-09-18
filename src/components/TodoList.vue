//Liste rendern
<script setup lang="ts">
import type {Todo} from "../models/todo.ts";
import TodoItem from './TodoItem.vue';

const props = defineProps({
  todos: {
    type: Array as () => Todo[],
    required: true,
  },
});

const emit = defineEmits<{
  (event: "toggle", id: number): void;
  (event: "delete", id: number): void;
}>();

</script>

<template>
  <div>
    <TodoItem v-for="item in props.todos" :key="item.id" :todo="item" @toggle="emit('toggle', $event)" @delete="emit('delete', $event)"/>
  </div>
</template>
