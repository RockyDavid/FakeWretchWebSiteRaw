<script setup lang="ts">
  import { ref, onMounted } from 'vue'
  import AlbumList from '../components/AlbumList/AlbumList.vue';
  import { Banner }  from '../components/Banner/Banner.ts';
  
  const comAlbumList = ref<AlbumList>(null);
  onMounted(() => {
    Banner.Show();
    comAlbumList.value.Show();
  });
</script>

<template>
  <main id="divMain" class="main">
    <AlbumList ref="comAlbumList" />
  </main>
</template>

<style>
  .main {
    position: relative;
    margin-bottom: 200px
  }
</style>
