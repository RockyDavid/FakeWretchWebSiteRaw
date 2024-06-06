<script setup lang="ts">
  import { ref, onMounted } from 'vue'
  import { Album, getPhotosAsync }  from '../../repositories/Album.ts';
  import { useAlbumPermitStore } from '../AlbumList/AlbumPermitStore.ts';
  import AlbumPlayer from '../AlbumList/AlbumPlayer.vue';
  import { Banner }  from '../Banner/Banner.ts';
  const store = useAlbumPermitStore();
  
  const props = defineProps({
    albumId: {
      type: Number,
      required: true
    },
    albumIdx: {
      type: Number,
      required: true
    }
  })

  const ablumPlayerContent = ref<HTMLDivElement>(null);
  const ablumContent = ref<HTMLDivElement>(null);
  
  const alblumPlayer = ref<AlbumPlayer>(null);
  const noPermit = !store.check(props.albumId);
  const idx = store.getIdx(props.albumId);
  var photos = ref<Album[]>([]);
  if (!noPermit) {
    getPhotosAsync(idx, 30).then((response: Album[]) => {
      photos.value = response;
    });
  }
  function onAlbumPlayerLoaded() { }
  function openAlbumPlayer(name: string, idx: number)
  {
    ablumPlayerContent.value.style.display = 'block';
    ablumContent.value.style.display = 'none';
    alblumPlayer.value.Open(name, props.albumIdx, idx);
  }
  function clickCloseAlbumPlayer()
  {
    ablumPlayerContent.value.style.display = 'none';
    ablumContent.value.style.display = 'grid';
    alblumPlayer.value.Close();
  }

</script>

<template>
    <div ref="ablumPlayerContent" class="ablum-player-content">
      <AlbumPlayer ref="alblumPlayer" @click:Close="clickCloseAlbumPlayer" @on:Loaded="onAlbumPlayerLoaded" />
    </div>
    <div v-if="!noPermit" ref="ablumContent" class="ablum-content bg:white/.3">
      <div class="photo-box" v-for="(photo, idx) in photos" :key="photo.id" @click="openAlbumPlayer(photo.name, idx)">
        <h2 class="photo-name"> {{ photo.name }} </h2>
        <img class="photo-img" :src="photo.image" />
      </div>
    </div>
    <h2 v-if="noPermit" class="color:red bg:black/.8 w:100% text-align:center"> 沒有密碼無法訪問 </h2>
</template>

<style  scoped>
  .ablum-player-content {
    position: relative;
    display: none;
    width: 100%;
    height: 775px;
  }
  .ablum-content {
    position: relative;
    display: grid;
    width: 100%;
    height: 775px;
    grid-template-columns: calc(20% - 2.4px) calc(20% - 2.4px) calc(20% - 2.4px) calc(20% - 2.4px) calc(20% - 2.4px);
    gap: 3px;
    grid-auto-rows: minmax(150px, auto);
    overflow-y: scroll;
    padding-bottom: 12px;
  }
  .photo-box {
    position: relative;
    display: block;
    width: 100%;
    height: 150px;
    padding: 5px;
    cursor: pointer;
    overflow: hidden;
    background-color: black;
  }
  .photo-box > .photo-name {
    margin: 0;
    padding: 3px;
    text-align: center;
    position: relative;
    display: block;
    overflow: hidden;
    text-wrap: nowrap;
    font-size: 12px;
    background:rgba(255,255,255,.3)
  }
  .photo-box > .photo-img {
    width: 100%;
    height: 100%;
    margin: 0;
    position: relative;
    display: block;
    overflow: hidden;
    object-fit: contain;
  }
</style>