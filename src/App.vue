<script setup lang="ts">
  import { ref, onMounted, onBeforeUnmount } from 'vue';
  import { RouterLink, RouterView } from 'vue-router';
  import Banner from './components/Banner/Banner.vue';
  import { Notify } from './components/Alert/Notify.ts';
  import MenuList from './components/MenuList/MenuList.vue';

  var divMain = ref<HTMLDivElement>(null);
</script>

<template>
  <div ref="divMain" class="rel max-width:1260 d:flex flex:col gap:10 p:0|50">
    <Alert />
    <header>
      <img alt="Wretch" class="logo" src="@/assets/wretch.png" />
      <Banner />
      <MenuList />
    </header>
    <RouterView />
  </div>
</template>

<style scoped>
  header {
    line-height: 1.5;
    text-align: center;
  }
  .logo {
    display: block;
    margin: 0 auto;
    height: 65px;
    position: relative;
  }
</style>
