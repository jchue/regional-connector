<script setup lang="ts">
import ButtonArrow from '@/components/ButtonArrow.vue';

defineProps(['buttonArrow', 'buttonLabel', 'last', 'layout']);

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
  <div v-if="layout === 'reset'" class="slide reset static m-2 min-h-5 p-0"></div>

  <div
    v-else-if="layout === 'header'"
    class="slide flex flex-col items-center justify-center px-8 text-center"
  >
    <slot></slot>

    <button v-if="!last" v-on:click="next" class="flex gap-2 items-center">
      {{ buttonLabel || 'Begin' }} <ButtonArrow v-bind:direction="buttonArrow" />
    </button>
  </div>

  <div
    v-else-if="layout === 'hero'"
    class="slide flex flex-col items-start justify-center bg-white font-semibold px-8 sm:px-16 !text-xl md:!text-3xl z-20"
  >
    <slot></slot>

    <button v-if="!last" v-on:click="next" class="flex gap-2 items-center">
      {{ buttonLabel || 'Next' }} <ButtonArrow v-bind:direction="buttonArrow" />
    </button>
  </div>

  <div
    v-else-if="layout === 'figure'"
    class="slide grid grid-cols-1 lg:grid-cols-[3fr_2fr] gap-8 items-center lg:p-0"
  >
    <figure class="mx-auto">
      <slot name="figure"></slot>
    </figure>

    <!-- Caption -->
    <div class="flex flex-col justify-start md:justify-center px-8 lg:px-0">
      <slot></slot>

      <button v-if="!last" v-on:click="next" class="next flex gap-2 items-center w-full md:w-auto">
        {{ buttonLabel || 'Next' }} <ButtonArrow v-bind:direction="buttonArrow" />
      </button>
    </div>
  </div>

  <div
    v-else-if="layout === 'map'"
    class="slide flex flex-col md:flex-row items-center justify-start md:justify-end"
  >
    <div class="h-[50vh] md:h-screen w-full md:w-7/12">
      <!-- Map space -->
    </div>

    <div
      class="relative flex flex-col justify-center min-h-[50vh] md:min-h-screen p-8 pb-16 shadow-none md:shadow-[-16px_0_16px_-16px_rgba(0,0,0,0.2)] w-full md:w-5/12 z-10"
    >
      <slot></slot>

      <div class="absolute bottom-0 left-0 md:left-auto right-0 p-2 flex gap-1 w-full md:w-auto">
        <button v-on:click="back" class="back w-full md:w-auto">Back</button>
        <button
          v-if="!last"
          v-on:click="next"
          class="next flex gap-2 items-center w-full md:w-auto"
        >
          {{ buttonLabel || 'Next' }} <ButtonArrow v-bind:direction="buttonArrow" />
        </button>
      </div>
    </div>
  </div>

  <div
    v-else-if="layout === 'figure-full'"
    class="slide grid grid-cols-1 grid-rows-[auto_auto_auto] gap-8 content-center justify-center bg-white z-20"
  >
    <!-- Caption -->
    <div class="flex flex-col justify-start md:justify-center px-8 lg:px-0">
      <slot name="caption-top"></slot>
    </div>

    <figure
      class="grid grid-col-1 sm:grid-cols-[repeat(auto-fit,minmax(250px,1fr))] gap-4 justify-items-center w-auto"
    >
      <slot name="figure"></slot>
    </figure>

    <!-- Caption -->
    <div class="flex flex-col justify-start md:justify-center px-8 lg:px-0">
      <slot name="caption-bottom"></slot>

      <button
        v-if="!last"
        v-on:click="next"
        class="self-center flex gap-2 items-center w-full md:w-auto"
      >
        {{ buttonLabel || 'Next' }} <ButtonArrow v-bind:direction="buttonArrow" />
      </button>
    </div>
  </div>

  <div v-else class="slide flex items-center bg-white px-8 z-20">
    <slot></slot>

    <button
      v-if="!last"
      v-on:click="next"
      class="self-center flex gap-2 items-center w-full md:w-auto"
    >
      {{ buttonLabel || 'Next' }} <ButtonArrow v-bind:direction="buttonArrow" />
    </button>
  </div>
</template>

<style>
.slide {
  @apply relative;
  @apply min-h-screen;
  @apply text-lg md:text-xl;
}
</style>
