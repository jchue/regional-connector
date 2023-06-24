<script setup lang="ts">
defineProps(['buttonLabel', 'last', 'layout']);

function back(e: Event) {
  const currentSlide = (e.currentTarget as HTMLElement).closest('.slide');
  if (currentSlide === null) return;

  const currentId = currentSlide.getAttribute('id');
  if (currentId === null) return;

  const currentIndex = parseInt(currentId.split('-')[1], 10);

  const nextIndex = currentIndex - 1;
  const nextId = `slide-${nextIndex}`;
  const nextSlide = document.getElementById(nextId);
  if (nextSlide === null) return;

  nextSlide.scrollIntoView();
  return;
}

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
  <div v-if="layout === 'reset'" class="slide reset">
  </div>

  <div v-else-if="layout === 'header'" class="slide header">
      <slot></slot>

      <button v-if="!last" v-on:click="next">{{ buttonLabel || 'Begin' }}</button>
  </div>

  <div v-else-if="layout === 'hero'" class="slide hero">
      <slot></slot>

      <button v-if="!last" v-on:click="next">{{ buttonLabel || 'Next' }}</button>
  </div>

  <div v-else-if="layout === 'figure'" class="slide figure">
    <figure>
      <slot name="figure"></slot>
    </figure>

    <div class="caption">
      <slot></slot>

      <button v-if="!last" v-on:click="next">{{ buttonLabel || 'Next' }}</button>
    </div>
  </div>

  <div v-else-if="layout === 'map'" class="slide map">
    <div class="map-space"></div>
    <div class="caption">
      <slot></slot>

      <div class="navigation">
        <button v-on:click="back" class="back">Back</button>
        <button v-if="!last" v-on:click="next" class="next">{{ buttonLabel || 'Next' }}</button>
      </div>
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

      <button v-if="!last" v-on:click="next">{{ buttonLabel || 'Next' }}</button>
    </div>
  </div>

  <div v-else class="slide default">
    <slot></slot>

    <button v-if="!last" v-on:click="next">{{ buttonLabel || 'Next' }}</button>
  </div>
</template>

<style>
.slide {
  min-height: 100vh;
  position: relative;
}

.reset {
  min-height: 20px;
  margin: 10rem;
  padding: 0;
  position: static;
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
  background-color: #fff;
  display: flex;
  flex-direction: column;
  font-size: 2rem;
  justify-content: center;
  padding: 0 2rem;
  z-index: 20;
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
  background-color: #fff;
  display: grid;
  gap: 2rem;
  grid-template-columns: 1fr;
  grid-template-rows: auto auto auto;
  align-content: center;
  justify-items: center;
  z-index: 20;
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

@media (max-width: 768px) {
  .caption {
    justify-content: flex-start;
  }
}

.map {
  align-items: center;
  display: flex;
  justify-content: flex-end;
  margin: 0;
}

.map .map-space {
  height: 100vh;
  width: 60%;
}

.map .caption {
  box-shadow: -16px 0 16px -16px rgba(0, 0, 0, 0.2);
  min-height: 100vh;
  padding: 2rem 2rem 4rem 2rem;
  position: relative;
  width: 40%;
  z-index: 10;
}

@media (max-width: 768px) {
  .map {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
  }

  .map .map-space {
    height: 50vh;
    width: 100%;
  }

  .map .caption {
    box-shadow: none;
    padding: 2rem 2rem 4rem 2rem;
    min-height: 50vh;
    width: 100%;
  }
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

.figure .navigation,
.map .navigation {
  bottom: 0;
  display: flex;
  gap: 2px;
  position: absolute;
  right: 0;
}

.figure button,
.map button {
  width: auto;
}

.figure-full button {
  align-self: center;
  width: auto;
}

@media (max-width: 768px) {
  .figure .navigation,
  .map .navigation {
    display: flex;
    width: 100%;
  }

  .figure button,
  .figure-full button,
  .map button {
    width: 100%;
  }
}

.default {
  background-color: #fff;
  display: flex;
  align-items: center;
  padding-left: 2rem;
  padding-right: 2rem;
  z-index: 20;
}
</style>
