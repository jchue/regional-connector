<script setup lang="ts">
defineProps(['line']);

function lineLetter(line: string) {
  if (line === 'e-old' || line === 'e-new') {
    return 'E';
  }

  return line.toUpperCase();
}
</script>

<template>
  <span v-bind:class='"icon " + line'>{{ lineLetter(line) }}</span>
</template>

<style scoped>
.icon {
  border-radius: 100%;
  display: inline-flex;
  font-weight: bold;
  height: 2rem;
  margin: 0 0.25rem;
  place-content: center;
  place-items: center;
  width: 2rem;
}

.a {
  background-color: #0072bc;
  color: #fff;
}

.b {
  background-color: #e3131b;
  color: #fff;
}

.c {
  background-color: #58a738;
  color: #fff;
}

.d {
  background-color: #a05da5;
  color: #fff;
}

.e,
.e-old {
  background-color: #5bc2e7;
  color: #000;
}

.k {
  background-color: #e96bb0;
  color: #000;
}

.l,
.e-new {
  background-color: #fdb913;
  color: #000;
}
</style>