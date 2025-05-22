<template>
  <!-- Hiển thị lỗi -->
  <div
    v-if="error"
    class="max-w-md mx-auto mt-10 p-6 bg-red-100 border border-red-300 rounded-xl text-center shadow"
  >
    <p class="text-red-700 font-medium">{{ error }}</p>
  </div>

  <!-- Đang tải -->
  <div
    v-else-if="loading"
    class="max-w-md mx-auto mt-10 p-6 bg-white border border-gray-200 rounded-xl text-center shadow"
  >
    <p class="text-gray-500 animate-pulse">Đang tải thông tin người dùng...</p>
  </div>

  <!-- Nội dung chính -->
  <div
    v-else
    class="max-w-md mx-auto mt-10 p-8 rounded-3xl shadow-xl text-center space-y-6
           bg-gradient-to-br from-white via-green-50 to-white border border-gray-200"
  >
    <!-- Avatar -->
    <div class="flex justify-center">
      <div class="w-28 h-28 rounded-full overflow-hidden border-4 border-green-500 shadow-md">
        <img
          :src="user.avtURL"
          @error="handleImageError"
          alt="Avatar người dùng"
          class="w-full h-full object-cover"
        />
      </div>
    </div>

    <!-- Vai trò -->
    <div>
      <p
        class="inline-block px-4 py-1 text-sm font-semibold text-green-800 bg-green-100 rounded-full shadow-sm"
      >
        🧑‍💼 Vai trò: {{ user.role }}
      </p>
    </div>

    <!-- Tên & headline -->
    <div>
      <h2 class="text-2xl font-bold text-gray-800">{{ user.name }}</h2>
      <p class="text-gray-500 mt-1 italic">{{ user.headline }}</p>
    </div>

    <!-- Location & Ngày tham gia -->
    <div class="text-sm text-gray-500 space-y-1">
      <p>
        <span class="font-medium text-gray-700">🌍 Địa điểm:</span>
        {{ user.location || 'Không rõ' }}
      </p>
      <p>
        <span class="font-medium text-gray-700">🕒 Tham gia:</span>
        {{ formatDate(user.createdAt) }}
      </p>
    </div>
  </div>
</template>




<script>
export default {
  name: 'UserInfo',
  data() {
    return {
      user: {
        memberID: '',
        name: 'Unknown',
        avtURL: 'https://i.pravatar.cc/300',
        createdAt: '',
        location: '',
        headline: 'No headline available',
        role: 'Member' //  Mặc định nếu không có
      },
      loading: true,
      error: null
    };
  },
  methods: {
    formatDate(dateString) {
      return new Date(dateString).toLocaleDateString('vi-VN', {
        year: 'numeric',
        month: 'long',
        day: 'numeric'
      });
    },
    handleImageError(event) {
      // Xử lý khi ảnh không tải được, thay bằng ảnh mặc định
      event.target.src = 'https://via.placeholder.com/300';
    }
  },
  async created() {
    try {
      const response = await fetch('/about.json'); // Sửa thành /data/about.json
      console.log('Response status:', response.status);
      if (!response.ok) {
        throw new Error(`HTTP error! Status: ${response.status}`);
      }
      const text = await response.text();
      console.log('Response text:', text);
      const data = JSON.parse(text);
      console.log('Parsed data:', data);
      this.user = data[0]; // Lấy bản ghi đầu tiên
      this.loading = false;
    } catch (err) {
      this.error = `Không thể tải thông tin người dùng: ${err.message}`;
      this.loading = false;
      console.error('Lỗi:', err);
    }
  }
};
</script>

<style >
/* Không cần CSS vì dùng Tailwind */
</style>