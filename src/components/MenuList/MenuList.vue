<script setup lang="ts">
  import { ref } from 'vue';
  import { Menu, getMenuAsync }  from '../../repositories/Menu.ts'

  const menus = ref<Menu>(new Menu(-1, '', ''));
  const menuElements = ref<HTMLAnchorElement[]>([]);

  getMenuAsync().then( response => { 
    menus.value = response; 
  });
  
</script>

<template>
  <div id="menu">
      <nav class="navMenu">
        <RouterLink 
          v-for="m in menus" 
          class="nav-menu" 
          :key="m.id" 
          :to="m.link" >{{ m.title }}</RouterLink>
      </nav>
  </div>
</template>

<style scoped>
    #menu {
      position: relative;
      display: block;
      width: 100%;
      margin: 6px 0;
      padding: 0 10px;
      border: 1px solid #FFF;
      background-color: rgba(0,0,0,.3);
      height: fit-content;
    }
    
    .navMenu {
      position: relative;
    }

    .navMenu > a {
      color: #fff;
      text-decoration: none;
      font-size: 1.2em;
      text-transform: uppercase;
      font-weight: 500;
      display: inline-block;
      width: 80px;
      height: 100%;
      cursor: pointer;
    }
  
    .navMenu > .router-link-active {
      background-color: white;
      color: black;
    }
  
</style>