//State + Logik

<script setup lang="ts">
import {computed, ref} from 'vue';
import type {Todo} from "../models/todo.ts";
import TodoList from "./TodoList.vue";

const filter = ref("ALL")

const newTodo = ref("")

// Beispiel Array
const todos = ref<Todo[]>([
  {
    id: 1,
    text: "Text1",
    done: false,
  },
  {
    id: 2,
    text: "Text2",
    done: false,
  },
  {
    id: 3,
    text: "Text3",
    done: true,
  },
])


const filteredTodos = computed(() => {
  if (filter.value === "ALL") {
    return todos.value;
  }

  if (filter.value === "OPEN") {
    return todos.value.filter(todo => !todo.done);
  }

  return todos.value.filter(todo => todo.done);
});

function addTodo(){
  const text = newTodo.value.trim() 

  if (text ===''){
    return
  }

  todos.value.push({
    id: Date.now(), text: newTodo.value, done: false
  })

  newTodo.value =''
}

function toggleTodo(id: number){
  todos.value.forEach(todo => {
    if (todo.id == id)
    {
      todo.done = !todo.done
    }
  });
}

  function deleteTodo(id: number) {
  todos.value = todos.value.filter(todo => todo.id !== id);
  }

</script>


// Gefilterte Todos
<template>
  <section id="list">
    <h1>Meine Todos</h1>

    <div id="filter">
      <button class="filter-button" :class="{ active: filter === 'ALL'}" @click="filter = 'ALL'">Alle</button>
      <button class="filter-button" :class="{ active: filter === 'OPEN'}" @click="filter = 'OPEN'">Offen</button>
      <button class="filter-button" :class="{ active: filter === 'DONE'}" @click="filter = 'DONE'">Erledigt</button>
    </div>

    <div>
      <p>Neue Todo:</p>
      <input type="text" @keyup.enter="addTodo" v-model="newTodo">
    </div>

    <TodoList :todos="filteredTodos" @toggle="toggleTodo" @delete="deleteTodo"/>

    

  </section>

</template>


