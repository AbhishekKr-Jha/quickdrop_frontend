
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
activeMenu:1,
isShareBtnActive:false
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
<div style="max-width: 1300px;" class="w-full min-h-screen p-5  flex flex-col  mx-auto bg-slate-900">   
  <HomePage :active_menu_id="activeMenu" v-show="activeMenu!==1" /> 

  <ProfilePage v-show="activeMenu==1"  :active_menu_id="activeMenu" @emit-toggle-share-btn="val => isShareBtnActive = val"   ref="profilePageRef"
    /> 


  <div class="fixed bottom-10 right-10 z-50 flex flex-col items-center     ">
    <img src="/web_logo.png" class="block mx-auto w-[200px] h-auto object-contain " alt="loading..." >
    <div v-show="!isShareBtnActive" class="mt-2 flex items-center justify-center gap-5">
<div @click="activeMenu=item.id" class="rounded-full size-12 bg-[#62CAEB]  flex items-center justify-center cursor-pointer font-semibold text-2xl text-white"   :class="{ 'border-2 border-black': activeMenu === item.id }" v-for="(item,index) in menuList" :key="index" :title="item.title">
  {{ item.text }}
</div>
</div>

<button @click="$refs.profilePageRef?.toggle_modal(true)" v-show="isShareBtnActive" class=" mt-2  justify-center items-center w-[120px] rounded-md px-4 py-2 text-white bg-[#1D4ED8] text-sm font-medium cursor-pointer "  >
Send </button>

  </div>


</div>
</template>
