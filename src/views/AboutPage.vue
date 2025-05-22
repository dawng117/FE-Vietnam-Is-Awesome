<template>
    <div class="about">
    <UserInfo />
  </div>
</template>

<script>
import UserInfo from '@/components/About/UserInfo.vue';

export default {
  name: 'AboutPage',
  components: { UserInfo }
};
</script>

<style scoped>
.about {
  padding: 20px;
}
</style>