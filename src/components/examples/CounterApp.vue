<script setup lang="ts">
import {computed, ref} from 'vue';

const count = ref(0)

function calculateDouble() {
  return count.value * 2
}

const doubleCount = computed(calculateDouble)


</script>

<template>
  <section id="center">
    <div>
      <h1>Task A - Counter</h1>
      Count is: {{ count }} <br />
      Double is: {{ doubleCount }}
    </div>
  </section>

  <section id="next-steps">
    <div id="center">
      <button type="button" class="counter" @click="count++">
        Increase
      </button>
    </div>
    <div id="center">
      <button type="button" class="counter" @click="count--">
        Decrease
      </button>
    </div>
  </section>

  <div class="ticks"></div>
  <section id="spacer"></section>
</template>
