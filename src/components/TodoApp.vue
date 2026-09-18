//State + Logik

<script setup lang="ts">
import {computed, ref} from 'vue';
import type {Todo} from "../models/todo.ts";
import TodoList from "./TodoList2.vue";

const filter = ref("All")

const newTodo = ref('')

// todos als reaktive Vue funktion definieren
// Array aus Todo Objekten erstellen
// Füllen des Arrays per Hand
const todos = ref<Todo[]>([
  {
    id: 1,
    text: 'Vue-Komponenten verstehen',
    done: false,
  },
  {
    id: 2,
    text: 'Aufgabe B fertigstellen',
    done: false,
  },
  {
    id: 3,
    text: 'README schreiben',
    done: true,
  },
])

const filteredTodos = computed(() => {
  if (filter.value == "All") {
    return [...todos.value]
  }
  else if(filter.value = "OPEN")
  {
    return todos.value.filter(todo => todo.done)
  }
  else {
    return todos.value.filter(todo => !todo.done)
  }
})

function addTodo(){
  todos.value.push({id: Date.now(), text: newTodo.value, done: false})
}

function toggleTodo(id: number){
  todos.value.forEach(todo => {
    if (todo.id == id)
    {
      todo.done = !todo.done
    }
  });


  // zum löschen: [...todos.value.filter(todo => todo.id != id)]

}

// oder todos.value.length +1
</script>


// hier dann filtered todos anzeigen

<template>
  <section id="center">
    <h1>Meine Todos</h1>
    <TodoList :todos="filteredTodos" @toggle="toggleTodo"/>
    <div>
      <input type="text" @keyup.enter="addTodo" v-model="newTodo">
    </div>
  </section>

</template>
