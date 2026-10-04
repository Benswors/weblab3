<script setup lang="ts">
import Users from './components/Users.vue'
</script>

<template>
  <div class="main-wrapper">
    <Users />
  </div>
</template>

<style>
html, body {
  margin: 0;
  padding: 0;
  width: 100%;
  min-height: 100vh;
  background-color: var(--main-bg, #f8fafc);
  color: var(--main-text, #0f172a);
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  box-sizing: border-box;
}

*, *::before, *::after {
  box-sizing: border-box;
}

.main-wrapper {
  width: 100%;
  min-height: 100vh;
}
</style>