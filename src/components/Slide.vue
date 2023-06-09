<script setup lang="ts">
defineProps(['last', 'layout']);

function next(e) {
  let el = e.currentTarget;
  while ((el = el.parentNode) && el.className.indexOf('slide') < 0);

  const currentId = el.getAttribute('id');
  const currentIndex = parseInt(currentId.split('-')[1], 10);

  const nextIndex = currentIndex + 1;
  const nextId = `slide-${nextIndex}`;
  const nextSlide = document.getElementById(nextId);

  nextSlide.scrollIntoView();
  
  return;
}
</script>

<template>
  <div v-if="layout === 'header'" class="slide header">
      <slot></slot>

      <button v-if="!last" v-on:click="next">Start</button>
  </div>

  <div v-else-if="layout === 'hero'" class="slide hero">
      <slot></slot>

      <button v-if="!last" v-on:click="next">Next</button>
  </div>

  <div v-else-if="layout === 'figure'" class="slide figure">
    <slot name="figure"></slot>

    <div class="caption">
      <slot></slot>

      <button v-if="!last" v-on:click="next">Next</button>
    </div>
  </div>

  <div v-else-if="layout === 'figure-full'" class="slide figure-full">
      <slot></slot>

    <button v-if="!last" v-on:click="next">Next</button>
  </div>

  <div v-else class="slide">
    <slot></slot>

    <button v-if="!last" v-on:click="next">Next</button>
  </div>
</template>

<style>
.slide {
  min-height: 100vh;
  margin-bottom: 4rem;
  position: relative;
}

.header {
  align-items: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.header button {
  position: static;
}

.hero {
  align-items: flex-start;
  display: flex;
  flex-direction: column;
  font-size: 2rem;
  justify-content: center;
  margin: 0 4rem;
}

.hero p {
  line-height: 1.2;
}

.hero button {
  position: static;
}

.figure {
  align-items: center;
  display: grid;
  gap: 2rem;
  grid-template-columns: 3fr 2fr;
}

.figure-full {
  display: grid;
  gap: 2rem;
  grid-template-columns: 1fr;
  grid-template-rows: auto auto auto;
  align-content: center;
  justify-items: center;
}

.figure-full .figure {
  display: grid;
  gap: 1rem;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  width: 100%;
}

.caption {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding-right: 2rem;
}

.figure button {
  bottom: 0;
  position: absolute;
  right: 0;
}
</style>
