<script setup lang="ts">
import { RouterView } from 'vue-router';
import GitHubIcon from '@/components/GitHubIcon.vue';

import '@fontsource/source-sans-pro/200.css';
import '@fontsource/source-sans-pro/200-italic.css';
import '@fontsource/source-sans-pro/400.css';
import '@fontsource/source-sans-pro/400-italic.css';
import '@fontsource/source-sans-pro/600.css';
import '@fontsource/source-sans-pro/600-italic.css';
import '@fontsource/source-sans-pro/700.css';
import '@fontsource/source-sans-pro/700-italic.css';
</script>

<template>
  <RouterView />

  <footer class="relative bg-white p-8 text-center text-base z-10">
    <p>
      This site is in no way affiliated with the Los Angeles County Metropolitan Transportation
      Authority.
    </p>

    <p>
      Leave comments and suggestions on
      <a
        href="https://github.com/jchue/regional-connector"
        target="_blank"
        class="inline-flex items-center gap-1 mx-1"
        ><GitHubIcon /> GitHub</a
      >.
    </p>
  </footer>
</template>
