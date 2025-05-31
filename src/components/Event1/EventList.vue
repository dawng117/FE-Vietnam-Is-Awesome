<template>
  <div class="space-y-6">
    <div v-if="error" class="text-red-600 bg-red-100 p-4 rounded-lg">
      {{ error }}
    </div>
    <div v-else-if="loading" class="text-gray-600 text-center py-4">
      Đang tải danh sách sự kiện...
    </div>
    <template v-else>
      <EventCard
        v-for="event in events"
        :key="event.id"
        v-bind="event"
      />
    </template>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import EventCard from './EventCard.vue';

const events = ref([]);
const loading = ref(true);
const error = ref(null);

onMounted(async () => {
  try {
    const response = await fetch('/event.json');
    if (!response.ok) throw new Error('Không thể tải dữ liệu sự kiện.');
    events.value = await response.json();
  } catch (err) {
    error.value = err.message;
  } finally {
    loading.value = false;
  }
});
</script>