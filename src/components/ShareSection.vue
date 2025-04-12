

<script >
import {  post_func } from '@/apiHelper/apiRequests';
import { send_otp_validator_func,otp_data_validator_func } from '@/utilities/validator_func';

export default{
name:'ShareSection',
data(){
        return{
fileSharingDetails:{
    userEmail:'',
    receiverEmail:'',
    title: '',
    message:'',
    sharedFile:''
},
otp:'',
isOtpInputVisible:false, 
otpToken:''
        }
    },
    methods:{
        async sendOtp  (){

  // ------validations
  let validationStatus =  send_otp_validator_func(this.fileSharingDetails)
    console.log(">>>>>",validationStatus)
    if(!validationStatus.success) return this.$toast.error(validationStatus.message) 

        const result =await post_func('/request_otp','application/json',{
            userEmail:this.fileSharingDetails.userEmail,
            receiverEmail: this.fileSharingDetails.receiverEmail,
            title: this.fileSharingDetails.title
        })
        if(result.success){
            this.isOtpInputVisible=true
            this.otpToken=result.response.token
        }
    },
    async verifyOtp  (){

        let validationStatus =  otp_data_validator_func({otp:this.otp,token:this.otpToken,...this.fileSharingDetails})
    console.log(">>>>>",validationStatus)
    if(!validationStatus.success) return this.$toast.error(validationStatus.message) 

        const formData = new FormData();
        formData.append('userEmail', this.fileSharingDetails.userEmail);
formData.append('receiverEmail',this.fileSharingDetails.receiverEmail )
formData.append('sharedFile',this.fileSharingDetails.sharedFile[0] ); 
formData.append('title', this.fileSharingDetails.title);
formData.append('message', this.fileSharingDetails.message);
formData.append('token',this.otpToken );
formData.append('otp',this.otp );

console.log(this.fileSharingDetails.sharedFile.length)

        const result =await post_func('/verify_otp','multipart/form-data',formData)
        if(result.success){
            this.isOtpInputVisible=false
        }
    },
    handleFileChange(e){
        console.log(e.target.files)
        this.fileSharingDetails.sharedFile=e.target.files
console.log(e)
    }
    },
}

</script>


<template>

<div style="padding: 20px ;padding-bottom: 40px;" class="w-full max-w-[420px] p-4  flex flex-col items-center justify-center gap-6  rounded-xl blur-background">
     <p class="text-2xl text-center">Share</p>
<div v-if="!isOtpInputVisible"  class=" w-[98%] max-w-[400px] p-5  flex flex-col items-center justify-center gap-4  ">


    <input type="text" v-model="fileSharingDetails.receiverEmail" name="receiverEmail" placeholder="Send to " required />

<input type="text"  v-model="fileSharingDetails.userEmail"  name="senderEmail" placeholder="Your Email " required />

<input type="text"  v-model="fileSharingDetails.title"   name="title" placeholder="Title" required />

<input type="text"  v-model="fileSharingDetails.message"  name="message" placeholder="Enter your message (Optional)" required />


<div style="margin: 8px 0;" class= " w-[90%] oveflow-hidden" > 
<input @change="handleFileChange" type="file" placeholder="File Upload" />
</div>

<button  @click="sendOtp"   type="button"  class=" mt-6 text-xl  mx-auto rounded-lg bg-[#62CAEB] text-white cursor-pointer" >Continue</button>
</div>

<div v-if="isOtpInputVisible"  class=" w-[98%] max-w-[400px] p-5  flex flex-col items-center justify-center gap-4  ">
    <input type="text" v-model="otp" name="otpt" placeholder="Enter your Otp" />

    <button @click="verifyOtp" type="button"  class=" mt-6 text-xl  mx-auto rounded-lg bg-[#62CAEB] text-white cursor-pointer" >Send</button>
</div>


</div>

</template>


<style>

input[type="text"]{
    width: 95%;
    padding:5px 10px ;
    border: none;
    border-bottom:1px solid gray ;
    outline: none;
    /* border-radius: 8px; */
margin: 0 auto;
}

</style>