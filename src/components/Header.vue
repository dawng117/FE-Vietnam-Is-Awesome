<template>
  <header class="bg-gray-800 text-white p-4">
    <h1 class="text-2xl font-bold">Vietnam Is Awesome</h1>
    <nav class="mt-4">
      <RouterLink to="/" class="mr-4 text-blue-300 hover:text-blue-100">Home</RouterLink>
      <RouterLink to="/about" class="text-blue-300 hover:text-blue-100">About</RouterLink>
      <RouterLink to="/challenges" class="text-blue-300 hover:text-blue-100">Challenges</RouterLink>
    </nav>
  </header>
</template>

<script setup>
import { RouterLink } from 'vue-router';
</script>

<style scoped>
header {
  display: flex;
  flex-direction: column;
  align-items: center;
}

nav a {
  text-decoration: none;
}

nav a.router-link-exact-active {
  color: white;
  font-weight: bold;
}
</style>