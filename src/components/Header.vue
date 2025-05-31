<template>
  <header class="bg-gradient-to-r from-gray-800 to-gray-900 text-white p-6 shadow-lg">
    <div class="container mx-auto flex flex-col md:flex-row justify-between items-center">
      <h1 class="text-3xl font-bold tracking-wider transform hover:scale-105 transition-transform duration-300">
        <span class="text-red-500">Vietnam</span> Is <span class="text-blue-500">Awesome</span>
      </h1>
      <nav class="mt-4 md:mt-0 space-x-6">
        <RouterLink 
          v-for="link in links" 
          :key="link.to" 
          :to="link.to"
          class="relative px-3 py-2 text-lg text-white hover:text-blue-400 transition-colors duration-300"
          :class="{ 'text-blue-500 font-semibold': isActiveRoute(link.to) }"
        >
          {{ link.text }}
          <span 
            class="absolute bottom-0 left-0 w-full h-0.5 bg-blue-400 transform scale-x-0 transition-transform duration-300"
            :class="{ 'scale-x-100': isActiveRoute(link.to) || isHoverRoute(link) }"
          ></span>
        </RouterLink>
      </nav>
    </div>
  </header>
</template>

<script setup>
import { RouterLink, useRoute } from 'vue-router';
import { computed, ref } from 'vue';

const route = useRoute();

const links = [
  { to: '/', text: 'Home' },
  { to: '/about', text: 'About' },
  { to: '/challenges', text: 'Challenges' }
];

// Kiểm tra đường dẫn đang active
const isActiveRoute = (path) => route.path === path;

// Nếu bạn muốn span underline cũng animate khi hover (mở rộng bằng scale-x),
// bạn có thể thêm một reactive ref để tracking hover state:
const hoverIndex = ref(null);
const isHoverRoute = (link, index) => hoverIndex.value === index;
</script>

<style scoped>
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(-10px); }
  to { opacity: 1; transform: translateY(0); }
}

header {
  animation: fadeIn 0.5s ease-out;
}

/* Mobile: chuyển nav từ ngang thành dọc */
@media (max-width: 768px) {
  nav {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1rem;
  }
  .space-x-6 > * {
    margin-left: 0 !important;
    margin-right: 0 !important;
  }
}
</style>
