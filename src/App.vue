<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router';
import GitHubIcon from '@/components/GitHubIcon.vue';
</script>

<template>
  <RouterView />

  <footer id="site-footer">
    <p>This site is in no way affiliated with the Los Angeles County Metropolitan Transportation Authority.</p>
    
    <p>Leave comments and suggestions on <a href="https://github.com/jchue/regional-connector" target="_blank"><span style="display: inline-flex; align-items: center; gap: 0.25rem; margin: 0 0.25rem;"><GitHubIcon /> GitHub</span></a>.</p>
  </footer>
</template>

<style>
#site-footer {
  font-size: 1rem;
  line-height: 1.3;
  padding: 2rem;
  text-align: center;
}

#site-footer p {
  margin-bottom: 1rem;
}

.title {
  font-size: 3rem;
  font-weight: bold;
  line-height: 1;
  margin: 0 0 2rem 0;
  text-align: center;
}

.footnote {
  bottom: 1rem;
  font-size: 1rem;
  left: 2rem;
  position: absolute;
}

.center {
  text-align: center;
}

.horizontal-list {
  padding-left: 0;
}

.horizontal-list li {
  list-style-type: none;
}

@media (min-width: 640px) {
  .horizontal-list li {
    border-right: 1px solid var(--color-text);
    display: inline-block;
    padding: 0 1.5rem;
  }

  .horizontal-list li:last-child {
    border-right: none;
  }
}

header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
