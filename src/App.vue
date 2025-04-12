
<script>
import HomePage from './views/home/HomePage.vue';
import ProfilePage from './views/home/ProfilePage.vue';
// import { ModalsContainer } from 'vue-final-modal'
 

export default{
  name:'App',
  data(){
    return{
menuList:[
  {
    id:0,
    text: 'S',
    title: 'Share'
  },
  {
    id:1,
    text: 'P',
    title: 'Profile'
  },
  {
    id:2,
    text: 'U',
    title: 'Upload'
  }
],
activeMenu:1
    }
  },
  components:{
    HomePage,
    ProfilePage,
    // ModalsContainer
  }
}

</script>


<template>
<div style="max-width: 1400px;" class="w-full min-h-screen mx-auto">   
  <HomePage :active_menu_id="activeMenu" v-show="activeMenu===0 
  || activeMenu===2" />

  <ProfilePage v-show="activeMenu===1"  /> 

  <!-- <ModalsContainer  /> -->

  <div class="fixed bottom-10 right-10 z-50 flex items-center justify-center gap-5 cursor-pointer font-semibold text-2xl text-white">
<div @click="activeMenu=item.id" class="rounded-full size-12 bg-[#62CAEB]  flex items-center justify-center "   :class="{ 'border-2 border-black': activeMenu === item.id }" v-for="(item,index) in menuList" :key="index" :title="item.title">
  {{ item.text }}
</div>
  </div>


</div>
</template>
