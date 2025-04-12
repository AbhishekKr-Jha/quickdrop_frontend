<script>
import { toRaw } from 'vue';
import { post_func } from '@/apiHelper/apiRequests';
import FileShareModal from '@/components/FileShareModal.vue';
import DisplayCard from '@/components/DisplayCard.vue';

 
export default{
    name:'ProfilePage',
    data(){
        return{
uploadTypes:[
    {id:0,title: 'All'},
    {id:1,title: 'Image'},
    {id:2,title: 'Files'},
    {id:3,title: 'Video'}
],
fileList:[],
imageList:[],
videoList:[],
documentList:[],
activeFilterDataTab:0,
others:[],
isModalOpen:false,
isMultiSelectFileOn:false,
selectedFiles:{
    selectedImageFiles:[],
    selectedVideoFiles:[],
    selectedDocumentFiles:[],
    selectedOtherFiles:[]
}
// selectedFile:null


        }
    },
    components:{
        DisplayCard,
        FileShareModal
    },
    methods:{
       
        async get_all_uploads(){
            const data=await post_func('/get_upload_list','application/JSON',{userEmail:"abhishekhp935@gmail.com"})
            if(data.success){
                this.fileList=data.response.url_list
             this.fileList.forEach(item=>{
const fileType=item.key.split('/')[2]
if(fileType=="image"){ this.imageList.push({fileType,...item}) }
else if(fileType=="video"){this.videoList.push({fileType,...item})}
else if(fileType=="application"){this.documentList.push({fileType,...item})}
else{this.others.push({fileType:"others",...item})}
                })
                console.log("imagelist length is",this.imageList)
                this.$toast.success(data.response.message)
                return
            }
            this.$toast.error('Sonething went wrong')  
        },
  
        handleMultiFileSelect(file) {
          console.log(file.fileData.fileType)
 if(file.selectedValue){
  console.log("the true is",toRaw(file.fileData))
    if (file.fileData.fileType === "image") {
    this.selectedFiles.selectedImageFiles.push(toRaw(file.fileData));
  } else if (file.fileData.fileType === "video") {
    this.selectedFiles.selectedVideoFiles.push(toRaw(file.fileData));
  } else if (file.fileData.fileType === "application") {
    this.selectedFiles.selectedDocumentFiles.push(toRaw(file.fileData));
  } else {
    this.selectedFiles.selectedOtherFiles.push(toRaw(file.fileData));
  }
 }
 else if(!file.selectedValue) {
  
  console.log("the false is",toRaw(file.fileData))
    if (file.fileData.fileType == "image") {
    this.selectedFiles.selectedImageFiles= this.selectedFiles.selectedImageFiles.filter(item=>item.key!==file.fileData.key);
  } else if (file.fileData.fileType == "video") {
    this.selectedFiles.selectedVideoFiles=this.selectedFiles.selectedVideoFiles.filter(item=>item.key!==file.fileData.key);
  } else if (file.fileData.fileType == "application") {
    this.selectedFiles.selectedDocumentFiles=this.selectedFiles.selectedDocumentFiles.filter(item=>item.key!==file.fileData.key);
  } else {
    this.selectedFiles.selectedOtherFiles=this.selectedFiles.selectedOtherFiles.filter(item=>item.key!==file.fileData.key);
  }
 }
console.log(this.selectedFiles.selectedDocumentFiles.length)
}
,
        handleSingleFileShare(file){
            console.log("te file is",toRaw(file))
      this.toggle_modal(true)
//   this.handleFileSelect(file)
if (file.fileType == "image") {
    this.selectedFiles.selectedImageFiles=[toRaw(file)];
  } else if (file.fileType == "video") {
    this.selectedFiles.selectedVideoFiles=[toRaw(file)];
  } else if (file.fileType == "application") {
    this.selectedFiles.selectedDocumentFiles=[toRaw(file)];
  } else {
    this.selectedFiles.selectedOtherFiles=[toRaw(file)];
  }
},
        
        toggle_modal(value){
this.isModalOpen=value
document.body.style.overflowY=value?"hidden":"auto" 
        },
        handleMultiSelectFileMode(value){
          // console.log(this.selectedFiles)
this.isMultiSelectFileOn=value
this.selectedFiles={
    selectedImageFiles:[],
    selectedVideoFiles:[],
    selectedDocumentFiles:[],
    selectedOtherFiles:[]
}
       },
      
    },
    mounted(){
        this.get_all_uploads()
    }

}

</script>


<template>

<div  class="w-full  p-5 space-y-16 ">
  
<div class="w-full   flex gap-5 items-center">
    <div @click="activeFilterDataTab=item.id" class="w-[100px] py-2 rounded-sm  text-center cursor-pointer" :class="activeFilterDataTab === item.id ? 'bg-red-900' : 'bg-[#62CAEB]'"
    v-for="item in uploadTypes" :key="item.id">
        {{ item.title }}
    </div>
    <div @click="handleMultiSelectFileMode(!isMultiSelectFileOn)" class="w-[100px] py-2 rounded-sm bg-[#62CAEB] text-center cursor-pointer" > MultiSelect </div>
    <div @click="toggle_modal(!isModalOpen)" class="w-[100px] py-2 rounded-sm bg-[#62CAEB] text-center cursor-pointer" > Share </div>
</div>

<!-- ----Images----- -->
<div v-show="imageList.length>0 && activeFilterDataTab==0 || activeFilterDataTab==1 " class="w-full space-y-8">
<div v-show="activeFilterDataTab==0" class="w-full flex justify-center items-center gap-8"><hr class="w-[35%] h-[2px]"><p class="text-2xl">Images</p><hr class="w-[35%] h-[2px]"></div>
<div class="w-full flex justify-center items-start gap-5 flex-wrap">
<DisplayCard v-for="item in imageList" :key="item" :data="item" @share-btn-clicked="handleSingleFileShare" :multiSelectActive="isMultiSelectFileOn" @file-selection-multi-mode="handleMultiFileSelect" /> 
</div>
</div>



<!-- ----------documents----- -->
<div v-show="documentList.length>0 && activeFilterDataTab==0 || activeFilterDataTab==2" class="w-full space-y-8">
<div v-show="activeFilterDataTab==0" class="w-full flex justify-center items-center gap-8"><hr class="w-[35%] h-[2px]"><p class="text-2xl">Documents</p><hr class="w-[35%] h-[2px]"></div>
<div class="w-full flex justify-center items-start gap-5 flex-wrap">
<DisplayCard v-for="item in documentList" :key="item" :data="item"  @share-btn-clicked="handleSingleFileShare" :multiSelectActive="isMultiSelectFileOn" @file-selection-multi-mode="handleMultiFileSelect"   />
</div>
</div>


<!-- ----------videos----- -->
<div v-show="videoList.length> 0 && activeFilterDataTab==0 || activeFilterDataTab==3" class="w-full space-y-8">
<div v-show="activeFilterDataTab==0" class="w-full flex justify-center items-center gap-8"><hr class="w-[35%] h-[2px]"><p class="text-2xl">Videos</p><hr class="w-[35%] h-[2px]"></div>
<div class="w-full flex justify-center items-start gap-5 flex-wrap">
<!-- <UploadCard v-for="item in videoList" :key="item" :data="item" /> -->
</div>
</div>





</div>

<div v-show="isModalOpen" @click="toggle_modal(false)" class=" w-full h-screen flex justify-center items-center fixed top-0 left-0 z-[60] blur-background">
    <FileShareModal  :files="selectedFiles" @close-modal-event="toggle_modal(!isModalOpen)"  />
</div>



</template>