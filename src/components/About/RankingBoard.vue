<template>
  <!-- Level indicator positioned outside the table -->
  <div v-if="currentUser" class="mb-3">
    <div class="flex items-center bg-amber-100 rounded-full px-3 py-1.5 shadow-sm inline-block">
      <span class="text-amber-600 mr-2">🏆</span>
      <span class="font-bold text-amber-700">{{ currentUser.level }}</span>
      <span class="text-amber-600 ml-2 mr-1">Level {{ currentUser.level }}</span>
    </div>
    <div class="text-gray-500 text-xs mt-1 ml-1">
      {{ currentUser.points }} points • {{ calculatePointsToNextLevel(currentUser.level, currentUser.points) }} to level up
    </div>
  </div>

  <div class="my-6 w-full bg-gradient-to-r from-blue-50 to-indigo-50 rounded-xl shadow-lg overflow-hidden">
    <h2 class="text-2xl font-bold text-center py-3 bg-indigo-600 text-white">BẢNG XẾP HẠNG</h2>

    <div class="p-3 overflow-x-auto">
      <table class="w-full border-collapse border border-indigo-200 text-center shadow-md table-fixed">
        <thead>
          <tr class="bg-indigo-500 text-white">
            <th class="border border-indigo-300 p-2 w-1/12 text-sm">Xếp hạng</th>
            <th class="border border-indigo-300 p-2 w-1/12 text-sm">Avatar</th>
            <th class="border border-indigo-300 p-2 w-4/12 text-sm">Tên</th>
            <th class="border border-indigo-300 p-2 w-3/12 text-sm">Level</th>
            <th class="border border-indigo-300 p-2 w-3/12 text-sm">Points</th>
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
              'hover:bg-indigo-100 transition-colors duration-150'
            ]"
          >
            <td class="border border-indigo-200 p-2">
              <div :class="[
                'rounded-full w-7 h-7 flex items-center justify-center mx-auto',
                index === 0 ? 'bg-yellow-400 text-yellow-800' : 
                index === 1 ? 'bg-green-500 text-white font-bold' : 
                index === 2 ? 'bg-amber-600 text-amber-900' : 'bg-indigo-100 text-indigo-800'
              ]">
                {{ index + 1 }}
              </div>
            </td>
            <td class="border border-indigo-200 p-2 relative">
              <img :src="user.avtURL" alt="avatar" 
                  :class="[
                    'w-10 h-10 rounded-full mx-auto shadow-sm',
                    index === 1 ? 'border-3 border-green-500 animate-pulse' : 'border-2 border-indigo-300'
                  ]" />
              <div v-if="index === 1" class="absolute -top-1 -right-1 bg-green-500 text-white text-xs rounded-full w-5 h-5 flex items-center justify-center">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-3 w-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
                </svg>
              </div>
            </td>
            <td class="border border-indigo-200 p-2 text-left pl-4">
              <div class="font-semibold text-sm" :class="{'text-green-700': index === 1, 'text-indigo-900': index !== 1}">
                {{ user.name }}
                <span v-if="index === 1" class="text-xs bg-green-500 text-white px-1 py-0.5 rounded-full ml-1">YOU</span>
              </div>
            </td>
            <td class="border border-indigo-200 p-2">
              <div class="flex flex-col items-center">
                <div :class="{'text-green-600 font-bold': index === 1, 'text-indigo-600 font-semibold': index !== 1}">
                  Level {{ user.level }}
                </div>
                <div class="text-xs text-gray-500 mt-1">
                  Cần {{ calculateNextLevelPoints(user.level) }} pts để lên Level {{ user.level + 1 }}
                </div>
                <div class="w-full bg-gray-200 rounded-full h-1.5 mt-1">
                  <div class="bg-indigo-600 h-1.5 rounded-full" :class="{'bg-green-500': index === 1}"
                       :style="{ width: calculateLevelProgress(user.points, user.level) + '%' }"></div>
                </div>
              </div>
            </td>
            <td class="border border-indigo-200 p-2 font-bold" :class="{'text-green-700': index === 1, 'text-indigo-800': index !== 1}">
              {{ user.points }} <span class="text-xs" :class="{'text-green-600': index === 1, 'text-indigo-600': index !== 1}">pts</span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'

const rankingData = ref([])

onMounted(async () => {
  try {
    const response = await fetch('/about.json')
    if (!response.ok) throw new Error('Không thể tải dữ liệu.')
    rankingData.value = await response.json()
  } catch (error) {
    console.error('Lỗi khi tải dữ liệu:', error)
  }
})

const sortedRankingData = computed(() =>
  [...rankingData.value].sort((a, b) => b.points - a.points)
)


const currentUser = computed(() => {
  return sortedRankingData.value.length > 1 ? sortedRankingData.value[1] : null
})


const calculateNextLevelPoints = (currentLevel) => {
  return (currentLevel + 1) * 100
}


const calculatePointsToNextLevel = (level, points) => {
  const nextLevelThreshold = calculateNextLevelPoints(level)
  return nextLevelThreshold - points
}


const calculateLevelProgress = (currentPoints, currentLevel) => {
  const nextLevelPoints = calculateNextLevelPoints(currentLevel)
  const currentLevelPoints = currentLevel * 100
  const pointsNeeded = nextLevelPoints - currentLevelPoints
  const pointsGained = currentPoints - currentLevelPoints
  
  
  const percentage = Math.min(Math.floor((pointsGained / pointsNeeded) * 100), 100)
  return percentage
}
</script>
