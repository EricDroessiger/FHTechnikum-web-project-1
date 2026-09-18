//einzelnes Todo mit Checkbox
<script setup lang="ts">
import { Todo } from "../models/todo";

const props = defineProps({
  todo: {
    type: Object as () => Todo,
    required: true,
  },
});


//Komponente kann toggle und delete Events senden und geben die ID des todos mit
const emit = defineEmits<{
  (event: "toggle", id: number): void;
  (event: "delete", id: number): void;
}>();

function deleteTodo(){
  emit("delete", props.todo.id);
}

</script>

//Toggle an die Liste weiter hoch geben
<template>
  <div>
    <input type="checkbox" name="id" :checked="props.todo.done" @toggle="$emit('toggle', props.todo.id)"> 
    <p>
      {{ props.todo.text }}
      <button type="button" @click="deleteTodo">Löschen</button>
    </p>
  </div>
  
</template>
