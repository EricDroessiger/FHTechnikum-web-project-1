//State + Logik

<script setup lang="ts">
import {ref} from 'vue';
import type {Todo} from "../models/todo.ts";
import TodoList from "./TodoList.vue";


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

</script>

<template>
  <section id="center">
    <h1>Meine Todos</h1>

    //Übergeben der der kompenente todo ihre Prop und inhalt der lokalen variable nehmen
    <TodoList :todos="todos"/>
  </section>

</template>
