<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router';
</script>

<template>
  <RouterView />

  <footer id="site-footer">
    This site is in no way affiliated with the Los Angeles County Metropolitan Transportation Authority.
  </footer>
</template>

<style>
#site-footer {
  font-size: 1rem;
  padding: 2rem 0;
  text-align: center;
}

p {
  line-height: 1.75;
  margin: 0 0 2rem 0;
}

p:last-child {
  margin-bottom: 0;
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
