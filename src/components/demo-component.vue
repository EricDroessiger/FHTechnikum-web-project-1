<script setup lang="ts">

// Component-First Thinking: diese Komponente ist die "Eltern-Komponente" für
// PersonItem und PersonForm. Sie hält den gemeinsamen Zustand (personList) und
// orchestriert die Kommunikation zwischen ihren Kind-Komponenten - Kinder kennen
// sich untereinander NICHT, sie kommunizieren nur über ihre gemeinsame Eltern-Komponente.
import type {maritalStatus, person} from "../models/person.ts";
import PersonItem from "./person-item.vue";
import PersonForm from "./person-form.vue";
import {ref} from "vue";

// ACHTUNG, Gegenbeispiel: das sind normale, NICHT-reaktive TypeScript-Variablen (nur "let").
// Sie werden zwar im Template angezeigt, aber wenn man sie später im Code ändern würde
// (z.B. someString = "..."), würde Vue das UI NICHT automatisch neu rendern.
// Für reaktive Werte braucht man ref() bzw. reactive() - siehe personList unten.
let someString: string = "Hello World"
let someNumber: number = 123

// Reaktive Variable: ref<person[]> macht das Array reaktiv und generisch typisiert
// (nur person-Objekte erlaubt). Jede Änderung (push, Zuweisung) lässt Vue das
// <ul> im Template automatisch neu rendern.
const personList = ref<person[]>([])

// TypeScript-Wiederholung: jeder Parameter ist typisiert (string, Date, maritalStatus)
// und auch der Rückgabewert der Funktion ist typisiert (": person"). Der Compiler
// meldet einen Fehler, falls z.B. ein falscher maritalStatus-String übergeben wird.
function generatePerson(firstName: string, lastName: string, birthdate: Date, maritalStatus: maritalStatus): person {
    return {
        firstName: firstName,
        lastName: lastName,
        birthdate: birthdate,
        maritalStatus: maritalStatus
    }
}

// Event-Handler: diese Funktion wird unten im Template an das "person-added"-Event
// von <person-form> gebunden. Sie ist das Gegenstück zum emit("personAdded", ...) in
// person-form.vue - so kommen Daten aus dem Kind wieder zurück zur Eltern-Komponente.
function addPerson(person: person): void {
    personList.value.push(person)
}

const father: person = generatePerson("Franz", "Moser", new Date('1956-05-01'), "married")
const mother: person = generatePerson("Sarah", "Moser", new Date('1954-04-01'), "married")
const son: person = generatePerson("Franz Jr", "Moser", new Date('1976-03-01'), "single")

personList.value.push(father, mother, son)

</script>

<template>
    <h1>{{someString}}</h1>
    <h2>{{someNumber}}</h2>
    <ul>
        <!-- Prop-Binding (":person"): reicht für jeden Durchlauf ein person-Objekt
             von dieser Eltern-Komponente nach unten an die Kind-Komponente PersonItem weiter. -->
        <person-item v-for="person in personList" :person="person"></person-item>
    </ul>

    <hr>
    <!-- Event-Listening ("@person-added"): hört auf das Event, das PersonForm per
         emit("personAdded", ...) sendet, und ruft dann addPerson() auf.
         Namenskonvention: im Template kebab-case ("person-added"),
         in defineEmits camelCase ("personAdded") - Vue wandelt automatisch um. -->
    <person-form @person-added="addPerson"></person-form>
</template>

<style scoped>

</style>
