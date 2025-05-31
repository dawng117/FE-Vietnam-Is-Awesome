<template>
  <!-- Level indicator -->
  <div v-if="currentUser" class="mb-4 sm:mb-6">
    <div class="flex items-center bg-amber-100 rounded-full px-3 py-2 sm:px-4 sm:py-2.5 shadow-sm inline-block">
      <span class="text-amber-600 mr-2 text-lg sm:text-xl">🏆</span>
      <span class="font-bold text-amber-700 text-sm sm:text-base">{{ currentUser.level }}</span>
      <span class="text-amber-600 ml-2 mr-1 text-sm sm:text-base">Level {{ currentUser.level }}</span>
    </div>
    <div class="text-gray-500 text-xs sm:text-sm mt-1 ml-1">
      {{ currentUser.points }} points • {{ calculatePointsToNextLevel(currentUser.level, currentUser.points) }} to level up
    </div>
  </div>

  <!-- Ranking Board (always table) -->
  <div class="my-6 w-full bg-gradient-to-r from-blue-50 to-indigo-50 rounded-xl shadow-lg">
    <h2 class="text-xl sm:text-2xl lg:text-3xl font-bold text-center py-3 bg-indigo-600 text-white">BẢNG XẾP HẠNG</h2>

    <div class="p-3 sm:p-4 lg:p-5 overflow-x-hidden">
      <table class="min-w-full border-collapse border border-indigo-200 text-center">
        <thead>
          <tr class="bg-indigo-500 text-white">
            <th class="border border-indigo-300 p-2 sm:p-3 w-auto sm:w-[6%] text-sm sm:text-base">Xếp hạng</th>
            <th class="border border-indigo-300 p-2 sm:p-3 w-auto sm:w-[8%] text-sm sm:text-base">Avatar</th>
            <th class="border border-indigo-300 p-2 sm:p-3 w-auto sm:w-[35%] text-sm sm:text-base">Tên</th>
            <th class="border border-indigo-300 p-2 sm:p-3 w-auto sm:w-[25%] text-sm sm:text-base">Level</th>
            <th class="border border-indigo-300 p-2 sm:p-3 w-auto sm:w-[16%] text-sm sm:text-base">Points</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(user, index) in sortedRankingData"
            :key="user.memberID"
            :class="[
              index === 1 ? 'bg-green-100 border-l-4 border-green-500' : 
                (index % 2 === 0 ? 'bg-white' : 'bg-indigo-50'),
              index < 3 ? 'font-bold' : '',
              'hover:bg-indigo-100 transition-colors'
            ]"
          >
            <td class="border border-indigo-200 p-2 sm:p-3">
              <div
                :class="[
                  'rounded-full w-7 sm:w-8 h-7 sm:h-8 flex items-center justify-center mx-auto',
                  index === 0 ? 'bg-yellow-400 text-yellow-800' : 
                  index === 1 ? 'bg-green-500 text-white font-bold' : 
                  index === 2 ? 'bg-amber-600 text-amber-900' : 'bg-indigo-100 text-indigo-800'
                ]"
              >
                {{ index + 1 }}
              </div>
            </td>
            <td class="border border-indigo-200 p-2 sm:p-3 relative">
              <img
                :src="user.avtURL"
                alt="avatar"
                :class="[
                  'w-8 sm:w-10 lg:w-12 h-8 sm:h-10 lg:h-12 rounded-full mx-auto shadow-sm',
                  index === 1 ? 'border-3 border-green-500' : 'border-2 border-indigo-300'
                ]"
                loading="lazy"
              />
              <div
                v-if="index === 1"
                class="absolute -top-1 -right-1 bg-green-500 text-white text-xs sm:text-sm rounded-full w-6 sm:w-7 h-6 sm:h-7 flex items-center justify-center"
              >
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 sm:h-5 w-4 sm:w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
                </svg>
              </div>
            </td>
            <td class="border border-indigo-200 p-2 sm:p-3 text-left pl-4 sm:pl-5">
              <div class="font-semibold text-sm sm:text-base" :class="{'text-green-700': index === 1, 'text-indigo-900': index !== 1}">
                {{ user.name }}
                <span v-if="index === 1" class="text-sm sm:text-base bg-green-500 text-white px-2 sm:px-2.5 py-0.5 sm:py-1 rounded-full ml-2">YOU</span>
              </div>
            </td>
            <td class="border border-indigo-200 p-2 sm:p-3">
              <div class="flex flex-col items-center">
                <div :class="{'text-green-600 font-bold': index === 1, 'text-indigo-600 font-semibold': index !== 1}" class="text-sm sm:text-base">
                  Level {{ user.level }}
                </div>
                <div class="text-xs sm:text-sm text-gray-600 mt-1">
                  Cần {{ calculateNextLevelPoints(user.level) }} pts để lên Level {{ user.level + 1 }}
                </div>
                <div class="w-full bg-gray-200 rounded-full h-1.5 mt-1">
                  <div
                    class="bg-indigo-600 h-1.5 rounded-full"
                    :class="{'bg-green-500': index === 1}"
                    :style="{ width: calculateLevelProgress(user.points, user.level) + '%' }"
                  ></div>
                </div>
              </div>
            </td>
            <td
              class="border border-indigo-200 p-2 sm:p-3 font-bold text-sm sm:text-base"
              :class="{'text-green-700': index === 1, 'text-indigo-800': index !== 1}"
            >
              {{ user.points }} <span class="text-xs sm:text-sm" :class="{'text-green-600': index === 1, 'text-indigo-600': index !== 1}">pts</span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';

const rankingData = ref([]);

onMounted(async () => {
  try {
    const response = await fetch('/about.json');
    if (!response.ok) throw new Error('Không thể tải dữ liệu.');
    rankingData.value = await response.json();
  } catch (error) {
    console.error('Lỗi khi tải dữ liệu:', error);
  }
});

const sortedRankingData = computed(() =>
  [...rankingData.value].sort((a, b) => b.points - a.points)
);

const currentUser = computed(() => {
  return sortedRankingData.value.length > 1 ? sortedRankingData.value[1] : null;
});

const calculateNextLevelPoints = (currentLevel) => {
  return (currentLevel + 1) * 100;
};

const calculatePointsToNextLevel = (level, points) => {
  const nextLevelThreshold = calculateNextLevelPoints(level);
  return nextLevelThreshold - points;
};

const calculateLevelProgress = (currentPoints, currentLevel) => {
  const nextLevelPoints = calculateNextLevelPoints(currentLevel);
  const currentLevelPoints = currentLevel * 100;
  const pointsNeeded = nextLevelPoints - currentLevelPoints;
  const pointsGained = currentPoints - currentLevelPoints;
  const percentage = Math.min(Math.floor((pointsGained / pointsNeeded) * 100), 100);
  return percentage;
};
</script>

<style scoped>
/* Không cần CSS tùy chỉnh vì Tailwind xử lý tốt */
</style>