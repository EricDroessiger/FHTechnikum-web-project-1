//Liste rendern
<script setup lang="ts">
import {ref} from 'vue';
import type {Todo} from "../models/todo.ts";
import TodoItem from './TodoItem2.vue';

const props = defineProps({
  todos: {
    type: Array as () => Todo[],
    required: true,
  },
});

const emit = defineEmits<{
  (event: "delete", id: number): void;
}>();

</script>

<template>
  <div>
    <TodoItem v-for="item in props.todos" :key="item.id" :todo="item" @delete="emit('delete', $event)"/>
  </div>
</template>
