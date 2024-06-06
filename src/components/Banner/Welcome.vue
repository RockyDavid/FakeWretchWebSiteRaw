<script setup lang="ts">
    import { Banner }  from '../../repositories/Banner.ts'
    import { Like }  from '../../repositories/Like.ts'
    const props = defineProps({
        banner: {
          type: [Object as PropType<Banner>, null],
          required: true
        },
        like: {
          type: [Object as PropType<Like>, null],
          required: true
        },
    })

    var isAlertShow = false;
    function subscript(event) {
        if(isAlertShow)
        {
            return;
        }
        isAlertShow = true;
        
        window.dispatchEvent(new CustomEvent('alertwarn', { detail: { message: '請先登入再訂閱！' }}));

        setTimeout(function(){
            isAlertShow = false;
        }, 5000);
    }

</script>

<template>
    <div id="welcome">
        <h1>歡迎來到我的無名小站</h1>
        <div class="margin-top:50"></div>
        <span class="description">請填入說明</span>
        <div class="likopt">
            <i @click="like.countOfLikes++"><b>{{ like.countOfLikes }}</b>愛的鼓勵</i>
            <i @click="subscript"><b>{{ like.countOfSubscript }}</b>訂閱站台</i>
        </div>  
    </div>
</template>

<style scoped>

    #welcome  {
      position: relative;
      display: inline-block;
      width: 100%;
      vertical-align: top;
      height: 100%;
    }
    
    #welcome h1 {
        font-size: 32px;
        margin: 0px;
        text-align: left;
    } 
    
    #welcome h1 a {
        color: var(--vt-text-dark);
    }
    
    #welcome .description {
        font-size:14px;
        z-index: 10;
        font-weight:normal;
        color: var(--vt-text-dark);
        position: relative;
        display: block;
        text-align: left;
    }
    .likopt {
      margin-top:35px;
    }
    .likopt i {
      display:block; 
      float: left; 
      width: 115px; 
      line-height: 24px;
      font-style: normal; 
      font-size: 12px; 
      text-align: center; 
      color: var(--vt-text-dark);
      cursor: pointer;
      background-color: black; 
        margin-right: 3px;
      }
      .likopt i span{ display: block; float: left; overflow: hidden; width: 25px; height: 25px; line-height: 25px; text-align: center; border-right: 1.5px solid #FFF; font-size: 15px;}
      .likopt i b{ margin: 0 5px; font-size: 14px;}
      .likopt i.actv span{ color: #797979; text-shadow:none; cursor:text;}
      .likopt i.actv{ color: #dddddd; cursor:text;}


</style>