<script setup lang="ts">
  import { ref, onMounted } from 'vue'
  import PostList from '../components/PostList/PostList.vue';
  import { Banner }  from '../components/Banner/Banner.ts';

  const comPostList = ref<PostList>(null);
  onMounted(() => {
    Banner.Show();
    comPostList.value.Show();
  });
</script>

<template>
  <main class="main">
    <PostList ref="comPostList" />
  </main>
</template>

<style scoped>
  .main {
    position: relative;
    display: block;
    margin-bottom: 200px
  }
</style>
