<template>
  <div class="wrapper">
    <span>Your name:</span><input v-model="username" type="text">
    <hr>
    <div class="scroll-area">
     <div class="message"
     v-for="message in messages"
     :key="message"
     >
       <h2>{{message.username}}</h2>
       <p>{{message.message}}</p>
     </div>
    </div>
     <hr>
     <div>
     <textarea v-model="message" type="text" class="input-message"></textarea>
     <button @click="submit">Send</button>
     </div>
     
  </div>
</template>

<script>

import Pusher from 'pusher-js';
export default {
 data(){
   return {
     username: '',
     message: '',
     messages: []
   }
 },
 mounted(){
   const pusher = new Pusher('c48d3a981b28ab2b7ee7', {
      cluster: 'eu'
    });
    const channel = pusher.subscribe('chat');
    channel.bind('message', data => {
      this.messages.push(data);
    });
 },
 methods: {
  async submit(){
   await fetch('http://localhost:8000/api/messages', {
     method: 'POST',
     headers: {'Content-Type': 'application/json'},
     body: JSON.stringify({
       username: this.username,
       message: this.message
     })
   });
   this.message = '';
   }
 }
}
</script>

<style>
.wrapper{
  text-align: center;
}
.scroll-area{
margin: 0 auto;
width: 75%; 
height: 400px;
background-color: #FFB273;
border: 1px solid #C1C1C1; 
overflow: auto; 
}
.message{
  text-align: center;
  width: 50%;
  min-height: 100px;
  background-color: #4fe0cf;
  margin: 15px auto;
  border-radius: 10px;
  box-shadow: 10px 10px 5px rgba(0,0,0,0.5);
  padding: 5px;
}
.input-message{
  width: 30%;
  height: 120px;
}
</style>
