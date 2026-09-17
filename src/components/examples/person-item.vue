<script setup lang="ts">
// Component-First Thinking: Diese Komponente ist bewusst klein und "dumm" gehalten -
// sie bekommt fertige Daten von außen (Props) und zeigt sie nur an. Sie kennt ihre
// Eltern-Komponente nicht und ändert die übergebenen Daten nicht selbst.
import type {person} from "../models/person.ts";

// TypeScript: hier wird der Typ der Props definiert - also welche Daten diese
// Komponente von der Eltern-Komponente ("demo-component.vue") entgegennehmen muss.
interface Props {
    person: person
}

// defineProps ist ein Vue-Compiler-Makro: es meldet die Props bei Vue an und liefert
// sie (dank <Props>) typsicher zurück. Props fließen IMMER nur von Eltern zu Kind (top-down).
const props = defineProps<Props>()
</script>

<template>
    <li>
        <!-- "person" ist hier direkt nutzbar (ohne "props."), weil defineProps() alle
             Props automatisch im Template zur Verfügung stellt. -->
        {{person.firstName}} {{person.lastName}},
        born in: {{person.birthdate.getFullYear()}},
        {{person.maritalStatus}}
    </li>
</template>

<style scoped>

</style>
