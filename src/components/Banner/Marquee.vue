<script setup lang="ts">

    const props = defineProps({
        welcome: {
          type: String,
          required: true
        }
    })
</script>

<template>
  <div class="marquee">歡迎來到這裡</div>
</template>

<style scoped>
  .marquee {
      width: 1024px;
      overflow: hidden;
      white-space: nowrap;
      box-sizing: border-box;
      padding: 10px 0;
      color: #FFF;
      text-align: center;
      position: absolute !important;
      top: 0;
      left: 0;
      z-index: 9999;
      font-size: 30px;
  }
</style>