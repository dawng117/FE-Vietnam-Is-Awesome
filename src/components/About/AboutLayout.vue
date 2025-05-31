<template>
    <div class="min-h-screen bg-gray-50 py-8">
      <!-- Wrapper -->
      <div class="container mx-auto px-4 max-w-7xl">
        <div class="grid gap-8 md:grid-cols-[1fr,1.5fr] items-start">
          <!-- User Info Block -->
          <section class="bg-white p-8 rounded-xl shadow-lg">
            <h2 class="text-2xl font-bold mb-6 text-gray-800">Thông tin cá nhân</h2>
            <UserInfo />
          </section>
    
          <!-- Ranking Board Block -->
          <section class="bg-white p-8 rounded-xl shadow-lg">
            <h2 class="text-2xl font-bold mb-6 text-gray-800">Bảng xếp hạng</h2>
            <RankingBoard />
          </section>
        </div>
      </div>
    </div>
</template>
  
<script setup>
import UserInfo from './UserInfo.vue'
import RankingBoard from './RankingBoard.vue'
</script>
  