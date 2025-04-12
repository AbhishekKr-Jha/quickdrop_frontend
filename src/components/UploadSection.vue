

<script>
import { post_func, put_func } from '@/apiHelper/apiRequests';

 
export default{
    name:'UploadSection',
    data(){
        return{
            accessCode:'',
            isAccessCodePassed:true,
            fileUploadingDetails:{
    userEmail:'abhishekhp935@gmail.com',
    fileName:'',
    fileType:'',
    contentType:'',
    sharedFile:'',
    fileExtension:''
},
        }
    },
    methods:{
        async verify_access_code(){
const result=await post_func('/access_upload_authorization','application/JSON',{access_code:this.accessCode})
console.log(result)
if(result.success) {
        this.isAccessCodePassed=true
    return this.$toast.success(result.response.message)
}
return this.$toast.error("Invalid Access Code!")
},

async request_uploading_url(){
    const result=await post_func('/upload_url','application/JSON',{
        userEmail:this.fileUploadingDetails.userEmail ,
        contentType :this.fileUploadingDetails.contentType ,
        fileType: this.fileUploadingDetails.fileType ,
        fileName:this.fileUploadingDetails.fileName+`.${this.fileUploadingDetails.fileExtension}`
    })

    if(result.success) {
        this.$toast.success(result.response.message)
        this.upload_file(result.response.presigned_url)
        return
    } 
return this.$toast.error(result.message)
},
async upload_file(presignedUrl){
const formData=new FormData()
formData.append('file',this.fileUploadingDetails.sharedFile)
const result=await put_func(presignedUrl,this.fileUploadingDetails.contentType,this.fileUploadingDetails.sharedFile)
if(result.success) return this.$toast.success("File Uploaded successflly")
return this.$toast.error("File Not Uploaded")
},
handleFileChange(e){
        console.log(e.target.files)
        const file_info=e.target.files[0].type.split('/')
        this.fileUploadingDetails.sharedFile=e.target.files[0]
        this.fileUploadingDetails.fileType=file_info[0]
        this.fileUploadingDetails.fileExtension=file_info[1]
console.log(e)
    }
}
        }
    


</script>


<template>

<div style="padding: 20px ;padding-bottom: 40px;" class="w-full max-w-[420px] p-4  flex flex-col items-center justify-center gap-4  rounded-xl blur-background">

    <!-- authenticatioin code -->  
<div v-show="!isAccessCodePassed" class="w-full  justify-center items-center ">
    <input type="text" v-model="accessCode" name="accessCode" placeholder=" Access Code" required />
    <button @click="verify_access_code" type="button"  class=" mt-6 text-xl  mx-auto rounded-lg bg-[#62CAEB] text-white" >Continue</button>
</div>


<!-- -----email validation--- -->
<div v-show="isAccessCodePassed" class="w-[98%] max-w-[400px] p-5  flex flex-col items-center justify-center gap-4 " >
<input type="text"  v-model="fileUploadingDetails.userEmail"  name="senderEmail" placeholder="Your Email " required />

<input type="text"  v-model="fileUploadingDetails.fileName"   name="filename" placeholder="File Name" required />

<div style="margin: 8px 0;" class= " w-[90%] oveflow-hidden" > 
<input @change="handleFileChange" type="file" placeholder="File Upload" />
</div>

<button  @click="request_uploading_url"   type="button"  class=" mt-6 text-xl  mx-auto rounded-lg bg-[#62CAEB] text-white cursor-pointer" >Continue</button>

</div>

<!--- ---files upload--- -->

</div>

</template>