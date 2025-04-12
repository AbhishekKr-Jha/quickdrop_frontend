

<script >
import { post_func } from '@/apiHelper/apiRequests';

export default{
    name:'FileShareModal',
props:{
    files:Array

},
data(){
    return{
fileSharingDetails:{
    userEmail:"abhishekhp935@gmail.com",
    receiverEmail: '',
    title: 'pahse 4 tesing',
    message:'optinal msg 4',
    // sharedFile:''
}
    }
},
methods:{
    
async share_file_upload_links(){
    const data=await post_func('/share_file_links','application/JSON',{
        userEmail:this.fileSharingDetails.userEmail,
        receiverEmail:this.fileSharingDetails.receiverEmail,
        title:this.fileSharingDetails.title,
        message:this.fileSharingDetails.message,
        imageFiles:this.files.selectedImageFiles ,
        videoFiles:this.files.selectedVideoFiles ,
        applicationFiles:this.files.selectedDocumentFiles , 
        otherFiles:this.files.selectedOtherFiles
    })
    if(data.success) { 
        this.$emit('close-modal-event')
        this.$toast.success(data.response.message)
    return
    }
this.$toast.error("something went wrong during multi share")
},
},
mounted(){
    console.log("the selectec image is",)
}
}

</script>

<template>
    <div @click.stop  class="w-[96%] max-w-[450px] p-5  flex flex-col justify-center items-center gap-4 rounded-xl ">

        <input type="text" v-model="fileSharingDetails.receiverEmail" name="receiverEmail" placeholder="Send to " class="outline-none" required />

<input type="text"  v-model="fileSharingDetails.title"   name="title" placeholder="Title" required />

<input type="text"  v-model="fileSharingDetails.message"  name="message" placeholder="Enter your message (Optional)" required />

<div class="w-full flex gap-4 justify-center items-center">

    
   
    <div  class="w-[130px] h-[100px] rounded-lg overflow-hidden   relative">
<img :src="files.selectedImageFiles[0]?.view_url" width="100%" height="100%" class="object-contain" >
<div class="w-full px-2 py-[2px] absolute bottom-0 right-0 bg-red-900 text-xs">
        <p class="truncate">{{ files.selectedImageFiles[0]?.key?.split('/')[3] }}</p>
    </div>
</div>

</div>

<button @click="share_file_upload_links"  type="button"  class=" mt-6 text-xl  mx-auto rounded-lg bg-[#62CAEB] text-white cursor-pointer" >Continue</button>


    </div>
</template>