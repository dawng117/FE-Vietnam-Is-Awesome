<template>
  <div class="min-h-screen bg-gray-50 py-8">
    <div class="container mx-auto px-4 max-w-5xl">
      <h2 class="text-3xl font-bold text-gray-800 mb-8">Upcoming Events</h2>
      <EventList />
    </div>
  </div>
</template>

<script setup>
import EventList from './EventList.vue';
</script>