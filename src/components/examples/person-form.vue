<script setup lang="ts">
import type {person} from "../models/person.ts";
import {computed, ref} from "vue";

// Events sind der Weg, wie ein Kind mit seiner Eltern-Komponente kommuniziert (bottom-up) -
// das Gegenstück zu Props (die nur top-down fließen). defineEmits<...> legt typsicher fest,
// welche Events diese Komponente auslösen kann und welchen Payload-Typ sie mitschicken.
const emit = defineEmits<{
    (e: "personAdded", person: person): void;
}>();


// Reaktive Variable: ref() macht dieses Objekt reaktiv. Ändert sich newPerson.value
// (z.B. durch v-model im Template), rendert Vue das Template automatisch neu.
// In <script> greift man mit ".value" zu, im <template> wird automatisch "entpackt".
const newPerson =  ref<person>({
    firstName: "",
    lastName: "",
    birthdate: new Date(),
    maritalStatus: "single",
})

function addPerson() {

    if(newPerson.value.firstName === "" || newPerson.value.lastName === "") {
        return;
    }

    // Event auslösen: sendet "personAdded" nach oben an die Eltern-Komponente, zusammen
    // mit dem Payload. { ...newPerson.value } erzeugt eine KOPIE des Objekts, damit das
    // spätere Zurücksetzen von newPerson die bereits gesendeten Daten nicht mehr verändert.
    emit("personAdded", { ...newPerson.value });

    // Formular zurücksetzen
    newPerson.value = {
        firstName: "",
        lastName: "",
        birthdate: new Date(),
        maritalStatus: "single",
    };
}

// Computed Variable: ein Wert, der aus anderen reaktiven Daten BERECHNET wird und sich
// automatisch aktualisiert, sobald sich newPerson.value.birthdate ändert (Caching inklusive).
// Normalerweise ist computed nur lesbar - hier ist es dank get()/set() SCHREIBBAR, damit
// v-model im Template direkt darauf zugreifen kann.
// Brücke zwischen Date (Model) und string (HTML input)
const birthdateString = computed({
    get() {
        // Date -> "yyyy-mm-dd" für das input-Feld
        return newPerson.value.birthdate.toISOString().split("T")[0];
    },
    set(value: string) {
        // "yyyy-mm-dd" -> Date fürs Model
        newPerson.value.birthdate = new Date(value);
    },
});

</script>

<template>
    <div>
        <input type="text" v-model="newPerson.firstName" placeholder="First Name" />
        <input type="text" v-model="newPerson.lastName" placeholder="Last Name" />
        <input type="date" v-model="birthdateString" placeholder="Birthdate" />
        <select v-model="newPerson.maritalStatus">
            <option>married</option>
            <option>single</option>
            <option>divorced</option>
        </select>

        <button @click="addPerson">Hinzufügen</button>
    </div>

</template>

<style scoped>

</style>
