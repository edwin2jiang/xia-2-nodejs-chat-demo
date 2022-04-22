<template>
  <div>
    <h1>socket.io演示客户端</h1>
    <h2>
      当前用户id: {{ currId }}
    </h2>
    <div>
      <input type="text" placeholder="发送消息" id="msg" v-model="msg">
      &nbsp;
      &nbsp;
      &nbsp;
      &nbsp;
      &nbsp;
      <input type="text" placeholder="发送给的用户id" id="toRoomId" v-model="toUserId" />
      &nbsp;
      &nbsp;
      &nbsp;
      &nbsp;
      &nbsp;
      <button id="sayTo" @click="sayTo">确定</button>
    </div>

    <div class="currentUsers">

    </div>
    <div id="text" ref="text">

    </div>
  </div>
</template>

<script >
export default {
  data() {
    return {
      msg: '',
      sendToId: '',
      currId: 0,
      toUserId: '',
      roomId: 0,
      socket: null,
    }
  },
  methods: {
    sayTo() {
      const { socket, msg, currId, toUserId } = this
      console.log('发送了消息', { "from": currId, "to": toUserId, "msg": msg })
      this.$refs.text.innerHTML += ("<p class='me'>" + '你:' + msg + "</p>");
      socket.emit("sayTo", { "from": currId, "to": toUserId, "msg": msg });
    }
  },
  created() {
    var socket = io.connect('127.0.0.1:3030');

    this.socket = socket

    // client-side
    socket.on("connect", () => {
      console.log(socket.id); // x8WIv7-mJelg7on_ALbx
      this.currId = socket.id
    });

    socket.on("disconnect", () => {
      console.log(socket.id); // undefined
    });

    // 当收到对方发来的数据后触发 message 事件
    socket.on('message', (data) => {
      console.log(this.$refs.text)
      this.$refs.text.innerHTML += ("<p class='you'>" + "别人:" + data + "</p>");
    });
  }
}

</script>



<style>
body {
  background-color: #f0f3f8;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#text {
  background-color: white;
  width: 400px;
  height: 600px;
  border-radius: 8px;
  margin: 20px auto;
  padding: 10px;
}

#text .me {
  text-align: right;
}

#text .you {
  text-align: left;
}
</style>
