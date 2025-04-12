
<script> 

export default{
    name:'DisplayCard',
    props:{
        data:Object,
        multiSelectActive:Boolean
    },
    data(){
        return{
            isSelected:false
        }
    },
    methods:{
        handleFileSelect(value){
            this.isSelected=value
            this.$emit('file-selection-multi-mode',{fileData:this.data,selectedValue:value})
        }
    },
    updated(){
      if(!this.multiSelectActive){
this.isSelected=false
      }
console.log("te value of multi select boolena is ",this.multiSelectActive)
    }
}
</script>



<template>
    <div  class="w-[250px] space-y-2 p-2 bg-green-500 rounded-lg">
    <div class="w-full flex items-center gap-2">
        <span class=" px-2 py-1 bg-blue-600 text-white font-medium rounded-sm text-xs ">pdf</span> 
        <p class="truncate">{{ data?.key?.split('/')[3] }}</p> 
    </div>
    <div  class="w-full h-[200px] rounded-lg overflow-hidden bg-black  relative">
        <!-- ---for images-- -->
<img v-show="data.fileType=='image'" :src="data.view_url" class="w-full h-full object-contain"  alt="load..." />

<!-- --i frame-------- -->
<iframe v-show="data.fileType=='application'" :src="data.view_url" width="100%" height="100%" ></iframe>

<!-- -------selecting multi file mode --------- -->
<div  v-show="multiSelectActive" @click="handleFileSelect(!isSelected)" class="w-full h-full absolute top-0 right-0 bg-red-900 flex justify-center items-center">

    <i v-show="isSelected" class="fa-solid fa-circle-check text-green-500 text-6xl"></i>

</div>

    <div v-show="!multiSelectActive" style="background: linear-gradient(to bottom,transparent, transparent, black);" class="w-full px-2 pr-4 py-1 flex justify-end gap-5 items-start absolute bottom-0 right-0 ">
<span  class="text-2xl cursor-pointer"><a :href="data.view_url" target="_blank"><i class="fa-solid fa-eye"></i></a></span>
<span @click="$emit('share-btn-clicked',data)"  class="text-2xl cursor-pointer bg-green-500"> <i class="fa-solid fa-share-nodes"></i></span>
<span class="text-2xl cursor-pointer"> <i class="fa-solid fa-trash-can"></i> </span>
</div> 
</div>
</div>
</template>