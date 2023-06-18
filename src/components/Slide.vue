<script setup lang="ts">
defineProps(['last', 'layout']);

function next(e: Event) {
  const currentSlide = (e.currentTarget as HTMLElement).closest('.slide');
  if (currentSlide === null) return;

  const currentId = currentSlide.getAttribute('id');
  if (currentId === null) return;

  const currentIndex = parseInt(currentId.split('-')[1], 10);

  const nextIndex = currentIndex + 1;
  const nextId = `slide-${nextIndex}`;
  const nextSlide = document.getElementById(nextId);
  if (nextSlide === null) return;

  nextSlide.scrollIntoView();
  return;
}
</script>

<template>
  <div v-if="layout === 'header'" class="slide header">
      <slot></slot>

      <button v-if="!last" v-on:click="next">Begin</button>
  </div>

  <div v-else-if="layout === 'hero'" class="slide hero">
      <slot></slot>

      <button v-if="!last" v-on:click="next">Next</button>
  </div>

  <div v-else-if="layout === 'figure'" class="slide figure">
    <figure>
      <slot name="figure"></slot>
    </figure>

    <div class="caption">
      <slot></slot>

      <button v-if="!last" v-on:click="next">Next</button>
    </div>
  </div>

  <div v-else-if="layout === 'figure-full'" class="slide figure-full">
    <div class="caption">
      <slot name="caption-top"></slot>
    </div>

    <figure>
      <slot name="figure"></slot>
    </figure>

    <div class="caption">
      <slot name="caption-bottom"></slot>

      <button v-if="!last" v-on:click="next">Next</button>
    </div>
  </div>

  <div v-else class="slide default">
    <slot></slot>

    <button v-if="!last" v-on:click="next">Next</button>
  </div>
</template>

<style>
.slide {
  min-height: 100vh;
  margin-bottom: 4rem;
  padding-bottom: 4rem;
  position: relative;
}

.header {
  align-items: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 0 2rem;
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
  padding: 0 2rem;
}

@media (min-width: 640px) {
  .hero {
    padding: 0 4rem;
  }
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
  grid-template-columns: 1fr;
}

.figure figure {
  margin: auto;
}

@media (min-width: 1024px) {
  .figure {
    grid-template-columns: 3fr 2fr;
    padding: 0;
  }
}

.figure-full {
  display: grid;
  gap: 2rem;
  grid-template-columns: 1fr;
  grid-template-rows: auto auto auto;
  align-content: center;
  justify-items: center;
}

.figure-full figure {
  display: grid;
  gap: 1rem;
  grid-template-columns: 1fr;
  width: 100%;
}

@media (min-width: 640px) {
  .figure-full figure {
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  }
}

.caption {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding-left: 2rem;
  padding-right: 2rem;
}

@media (min-width: 1024px) {
  .caption {
    padding-left: 0;
  }

  .figure .caption {
    padding-right: 2rem;
  }

  .figure-full .caption {
    padding-right: 0;
  }
}

.figure button,
.figure-full button {
  width: 100%;
}

@media (min-width: 640px) {
  .figure button {
    bottom: 0;
    position: absolute;
    right: 0;
    width: auto;
  }

  .figure-full button {
    align-self: center;
    width: auto;
  }
}

.default {
  display: flex;
  align-items: center;
  padding-left: 2rem;
  padding-right: 2rem;
}
</style>
