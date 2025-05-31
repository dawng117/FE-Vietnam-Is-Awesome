<template>
  <div class="bg-white rounded-lg shadow-md p-6 mb-4 hover:shadow-xl transition-shadow duration-300">
    <div class="flex justify-between items-start mb-4">
      <div class="flex items-start">
        <div class="bg-gray-100 rounded-lg p-3 text-center mr-4 min-w-[60px]">
          <div class="text-2xl font-bold text-gray-800">{{ day }}</div>
          <div class="text-sm text-gray-600 uppercase">{{ month }}</div>
        </div>
        <div>
          <h3 class="text-xl font-semibold text-gray-800">{{ title }}</h3>
          <p class="text-gray-600">{{ time }}</p>
          <p class="text-sm text-gray-500 mt-1">Organized by: {{ organizer }}</p>
        </div>
      </div>
      <div class="flex flex-col items-end">
        <span class="text-lg font-semibold text-gray-700 mb-2">${{ price }} USD</span>
        <button 
          class="bg-red-500 hover:bg-red-600 text-white font-semibold py-2 px-4 rounded transition-colors duration-300"
          :disabled="status !== 'open'"
          :class="{ 'opacity-50 cursor-not-allowed': status !== 'open' }"
        >
          {{ status === 'open' ? 'RSVP' : 'Closed' }}
        </button>
        <p class="text-sm text-gray-500 mt-2">
          {{ currentParticipants }}/{{ maxParticipants }} participants
        </p>
      </div>
    </div>
    <div class="mt-4">
      <div class="flex items-start">
        <div class="w-48 h-32 mr-4">
          <img :src="mapImage" alt="Location Map" class="w-full h-full object-cover rounded shadow" />
        </div>
        <div>
          <h4 class="font-semibold text-gray-800 mb-2">{{ location }}</h4>
          <p class="text-gray-600">{{ address }}</p>
          <p class="text-gray-600 mt-2">{{ description }}</p>
          <a 
            :href="mapUrl" 
            target="_blank"
            class="text-blue-500 hover:text-blue-600 mt-2 inline-block transition-colors duration-300"
          >
            Xem bản đồ
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  id: {
    type: Number,
    required: true
  },
  day: {
    type: String,
    required: true
  },
  month: {
    type: String,
    required: true
  },
  title: {
    type: String,
    required: true
  },
  time: {
    type: String,
    required: true
  },
  price: {
    type: Number,
    required: true
  },
  location: {
    type: String,
    required: true
  },
  address: {
    type: String,
    required: true
  },
  mapUrl: {
    type: String,
    required: true
  },
  mapImage: {
    type: String,
    required: true
  },
  organizer: {
    type: String,
    required: true
  },
  description: {
    type: String,
    required: true
  },
  maxParticipants: {
    type: Number,
    required: true
  },
  currentParticipants: {
    type: Number,
    required: true
  },
  status: {
    type: String,
    required: true,
    validator: (value) => ['open', 'closed'].includes(value)
  }
});
</script>