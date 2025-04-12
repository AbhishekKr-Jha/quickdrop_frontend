
<script>
import ShareSection from '@/components/ShareSection.vue';
import UploadSection from '@/components/UploadSection.vue';

export default{
    name:'HomePage',
    props:{
active_menu_id:Number
    },
    components:{
        ShareSection,
        UploadSection
    },
    mounted(){
        console.log("the id value is", typeof this.active_menu_id,
        this.active_menu_id
        )
    }
}
</script>


<template>
    
<div style="padding: 30px;" class="w-full  flex relative justify-center gap-8 bg-slate-900">
    <div class=" w-[85%] md:w-1/2 mx-auto  min-h-screen flex justify-center items-center  relative z-20">
        <ShareSection v-show="active_menu_id===0" />
        <UploadSection  v-show="active_menu_id===2" />
    </div>


    <div class="w-full min-h-screen md:w-1/2 flex justify-center items-center absolute md:relative z-10 ">
        <div class=" size-[80%] md:size-[400px]">
<img src="https://cdni.iconscout.com/illustration/premium/thumb/file-sharing-network-illustration-download-in-svg-png-gif-formats--digital-online-document-exchange-electronic-transfer-pack-files-folders-illustrations-10288813.png?f=webp" class="w-full h-full object-contain"  alt="loading..."/>      
  </div>
    </div>


</div>
</template>