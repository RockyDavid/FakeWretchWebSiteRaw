<script setup lang="ts">
  import { ref } from 'vue'
  import { useRouter } from 'vue-router';
  import { Notify } from '../Alert/Notify.ts';
  import { Album }  from '../../repositories/Album.ts';
  import { useAlbumPermitStore } from './AlbumPermitStore.ts';
  const store = useAlbumPermitStore();

  const router = useRouter();
  const props = defineProps({
    album: {
      type: [Object as PropType<Album>, null],
      required: true
    },
    idx: {
      type: Number,
      required: true
    } 
  });
  function goToAlbum()
  {
    if (!props.album.isPublic) {
      const inputPwd = (document.getElementById(`ablumpwd${props.album.id}`) as HTMLInputElement)?.value;
      if (props.album.password !== inputPwd) {
        Notify.Warn('密碼錯誤');
        store.add(props.album.id, props.idx, false);
        return;
      }
    }
    store.add(props.album.id, props.idx, true);
    router.push('/albumdetail/' + props.album.id + '/' + props.idx);
  }
</script>

<template>
  <div class="album-list-cell w:100% h:190px rel bg:black/.3 overflow:hidden">
    <div class="ablum-name rel block w:100% h:30px overflow:hidden bg:black/.3 color:white"><div> {{ props.album.name }} </div></div>
    <div class="img" v-if="!props.album.isPublic"><div>私人相簿</div></div>
    <img v-if="props.album.isPublic" :src="props.album.cover"/>
    <div class="ablum-password" v-if="!props.album.isPublic">
      <i class="fa fa-key"></i>
      <input type="password" class="album-password-input" :id="'ablumpwd' + props.album.id" />
      <button class="btn" @click="goToAlbum">進入相簿</button>
    </div>
    <div class="ablum-enter" v-if="props.album.isPublic">
      <button class="btn" @click="goToAlbum">進入相簿</button>
    </div>
  </div>
</template>

<style scoped>
  .album-list-cell {
  }
  .album-list-cell > .ablum-name > div {
    white-space: nowrap;
    text-align:center;
  }
  .album-list-cell > img {
    display: block;
    width: 100%;
    height: 130px;
  }
  .ablum-enter {
    text-align: center;
    bottom: 1px;
    position: absolute;
    left: calc(50% - 42px);
  }
  .album-list-cell > .img {
    width: 100%;
    height: 130px;
    background-color: darkred;
  }
  .album-list-cell > .img > div {
    padding-top: 43px;
    font-size: 30px;
    letter-spacing: 15px;
    display: block;
    padding-left: 30px;
  }
  .ablum-password {
    margin: 0;
    display: inline-block;
    width: 100%;
    overflow: hidden;
    padding: 0;
  }
  .ablum-password > .fa {
    position: absolute;
    margin-top: 0px;
    margin-left: 0px;
    color: lightyellow;
    padding-top: 6px;
    padding-left: 3px;
  }
  .ablum-password > input {
    width: 114px;
    font-size: 14px;
    vertical-align: bottom;
    margin-left: 20px;
    color: #FFF;
    margin-top: 4px;
  }
  .ablum-password > .btn {
    position: absolute;
    right: 9.5px;
    margin-top: 3px;
  }

  .ablum-enter {
    text-align: center;
  }
  .btn {
    margin: 3px auto;
    display: inline-block;
    text-align: center;
  }
  .btn:hover {
    cursor: pointer;
  }
</style>