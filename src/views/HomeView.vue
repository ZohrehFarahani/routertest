<template>
  <main role="main" class="container">

          <div v-for="item in  messageList" :key="item.id"  v-bind:class="[item.id == 1 ? 'alert alert-secondary bg-light d-flex justify-content-between' : 'alert alert-primary text-left']"   >
          {{item.message}}</div>
        
            

 
  
    <div class="input-group   fixed-bottom " style="margin-bottom: 15px; margin-right: 15px;"  >
    <input  type="text" class="form-control" v-model="mess" placeholder="لطفا تایپ نمایید" v-on:keyup.enter="sendMessage()"/>
 
<div class="input-group-prepend">
   <button v-on:click="sendMessage()" class="btn btn-primary ">ارسال</button>
  </div>
</div>


   <br>
    <span v-show="visibale">ارتباط با موفقیت انجام شد</span>
    <br>
     
          <span v-text="result"></span>
     
          

  </main>
</template>

<script>

import 'bootstrap-vue/dist/bootstrap-vue.css';
import 'bootstrap/dist/css/bootstrap.css';
export default {
  name: 'HomeView',
  data: function() {
    return {
      connection: null,
      messageList:[],
      show:null,
      visibale:false,
      result:null
    }
  },
  methods: {
      sendMessage: function() {  
      this.connection.send(this.mess)
      this.messageList.push({id:2,message:this.mess})
    },
     connect : function() {
     this.connection = new WebSocket("wss://demo.piesocket.com/v3/channel_1?api_key=VCXCEuvhGcBDP7XhiJJUDvR1e1D3eiVjgZ9VRiaV&notify_self")
     this.connection.onmessage =(message)=> {
     console.log(message.data)
     this.messageList.push({id:1,message:message.data})
    
    }

    this.connection.onopen = function(event) {    
      console.log(event)
      console.log ("ارتباط با موفقیت انجام شد"+event.data)
      this.show ="ارتباط با موفقیت انجام شد"
      this.visibale = true
     
    }
    this.connection.onerror = function(err) {
      this.show = 'خطا در سوکت ', err.message, 'سوکت بسته شده است'
      this.connection.close()
     }
    this.connection.onclose = function(err) {
        console.log('سوکت بسته شده است . ارتباط مجدد'+err.message)
         this.show ='سوکت بسته شده است . ارتباط مجدد'+err.message
       setTimeout(function() {
       this.connect()
        }, 1000)
     }
  }
  },
  created: function() {
      this.connect()
  },
  
}
</script>
