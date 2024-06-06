<script setup lang="ts">
  import { User }  from '../../repositories/User.ts'
  const props = defineProps({
      whocome: {
        type: [Object as PropType<User>, null],
        required: true
      },
  })
</script>

<template>
  <div id="whocome">
    <img v-for="child in props.whocome" src="@/assets/noimg.webp" />
  </div>
</template>

<style scoped>
    #whocome {
      position: relative;
      width: 100%;
      display: block;
      padding: 15px 150px;
    } 
    #whocome img {
      display: inline-block;
      max-height:72px;
      border: 3px #000 solid;
      cursor: pointer;
    } 
</style>