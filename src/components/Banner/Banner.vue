<script setup lang="ts">
  import { ref, onMounted, onBeforeUnmount } from 'vue'
  import Welcome  from './Welcome.vue'
  import WhoCome  from './WhoCome.vue'
  import Marquee  from './Marquee.vue'
  import { Banner, getBannerAsync }  from '../../repositories/Banner.ts'
  import { Like, getLikeAsync }  from '../../repositories/Like.ts'
  import { User, getWhoComes9Async }  from '../../repositories/User.ts'
  var whocome = ref<User[]>([new User(-1, '')]);
  getWhoComes9Async().then( response => { 
    whocome.value = response 
  });


  var bannerUI = ref<HTMLElement>(null);
  var banner = ref<Banner>(new Banner(-1, '', ''));
  var like = ref<Like>(new Like(-1, '', ''));
  
  getBannerAsync().then( response => { 
    banner.value = response 
  })
  getLikeAsync().then( response => { 
    like.value = response 
  })

  const handleBannerShow = (event: CustomEvent) => {
    bannerUI.value.style.display = 'grid';
  };
  const handleBannerHide = (event: CustomEvent) => {
    bannerUI.value.style.display = 'none';
  };

  onMounted(() => {
    bannerUI.value = document.getElementById('banner') as HTMLElement;
    window.addEventListener('bannershow', handleBannerShow as EventListener);
    window.addEventListener('bannerhide', handleBannerHide as EventListener);
  });

  onBeforeUnmount(() => {
    window.removeEventListener('bannershow', handleBannerShow as EventListener);
    window.removeEventListener('bannerhide', handleBannerHide as EventListener);
  });
    
</script>

<template>
  <div id="banner" class="bg:white/.3 color:white">
    <WhoCome :whocome="whocome" />
    <Welcome :key="banner.id" :banner="banner" :like="like" />
  </div>
</template>

<style scoped>
  #banner {
    width: 100%;
    position: relative;
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 3px;
    overflow: hidden;
    height: 250px;
  }
</style>