<template>
  <div id="app">
    <Container>
      <ChatWindow @send-message="sendMessage">
        <ChatMessage v-for="(el, i) in data" :key="i" :username="el.author" :datetime="el.datetime" :text='el.text'></ChatMessage>
      </ChatWindow>
    </Container>
  </div>
</template>

<script>
  import axios from 'axios'
  import Container from './components/Container.vue'
  import ChatMessage from './components/ChatMessage.vue'
  import ChatWindow from './components/ChatWindow.vue'
  export default {
    data() {
      return {
        data: []
      }
    },
    methods: {
      sendMessage(obj) {
        axios.post('http://188.225.47.187/api/chat/sendmessage.php', {
          author: obj.nickname,
          text: obj.message,
        })
        .then((responce)=>{
          console.log('rrr', responce)
          this.getMessage()
        })
      },
      getMessage() {
        axios.get('http://188.225.47.187/api/chat/getmessages.php')
        .then((responce)=>{
          this.data = responce.data
          console.log('responce', responce.data)
        })
      }
    },
    name: 'app',
    components: {
      Container,
      ChatMessage,
      ChatWindow
    },
    mounted(){
        setInterval(()=>{
          this.getMessage()
        }, 3000)
    }
  }
</script>

<style>
  body {
    margin: 0;
    background-color: #f9f9fa;
  }
</style>
