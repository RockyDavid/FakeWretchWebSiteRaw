<script setup lang="ts">
  import { ref, onMounted, nextTick  } from 'vue'
  import { Album, getPhotosAsync }  from '../../repositories/Album.ts';
  import { common }  from '../../helper/common.ts';

  const emit = defineEmits(['click:Close','on:Loaded']);
  const divPlayerBox = ref<HTMLDivElement>(null);

  const divPhoto = ref<HTMLDivElement>(null);
  const imgPhoto = ref<HTMLImageElement>(null);
  const photos = ref<Album[]>([]);
  const ablumName = ref<string>('');
  const photoIndex = ref<number>(0);
  const isFirstImageLoaded = ref<boolean>(true);
  const isLoaded = ref(false);
  
  const Open = (name: string, albumIdx: number, idx: number) => {
    const max = common.RandomNum(5, 60);
    photoIndex.value = idx;
    getPhotosAsync(albumIdx, max).then((response: Album[]) => {
      photos.value = response;
      ablumName.value = name;
      divPlayerBox.value.style.display = 'block';
      isFirstImageLoaded.value = true;
      loadImage();
      emit('on:Loaded');
    });
  };
  
  const Close = (idx: number) => {
    photos.value = [];
    divPlayerBox.value.style.display = 'none';
  };
  defineExpose({ Open, Close });

  function close()
  {
    emit('click:Close');
  }

  function loadImage() {
    imgPhoto.value.src = photos.value[photoIndex.value].image;
  }


  function doPrev() {
    photoIndex.value = (photoIndex.value === 0) ? photos.value.length - 1 : photoIndex.value - 1;
    loadImage();
  }
  function doNext() {
    photoIndex.value = (photoIndex.value === photos.value.length - 1) ? 0 : photoIndex.value + 1;
    loadImage();
  }
</script>

<template>
  <div ref="divPlayerBox" class="album-player abs z-999 none w:100% h:100% top:0 left:0 bg:black/1 overflow:hidden">
    <div class="header pl:12px bg:white/.3">
      <div class="name"> {{ `${ablumName} - ${photoIndex+1} / ${photos.length}`  }} </div>
      <button class="next rel" @click="close">返回相簿</button>
      <button class="close rel" @click="doNext">下一張</button>
      <button class="prev rel" @click="doPrev">上一張</button>
    </div>
    <div ref="divPhoto" class="photo rel block w:100% h:100%"><img ref="imgPhoto" /></div>
  </div>
</template>

<style scoped>
  .z-999 { z-index: 999; }
  .name {
    display: inline-block;
    margin-right: 6px;
  }
  .prev,
  .next,
  .close   {
    float: right;
    cursor: pointer;
    margin-right: 6px;
  }
  .photo > img {
    height: 100%;
    width: 100%;
    margin: 0;
    position: relative;
    display: block;
    overflow: hidden;
    object-fit: contain;
  }
</style>