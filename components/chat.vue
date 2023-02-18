<template>
  <div class="app">
    <header><p>WebSocket Chat Test</p></header>
    <div class="chat-window">
      <div class="input-group">
        <textarea
          @keydown.enter.exact.prevent="sendMessage"
          @keydown.enter.shift.exact.prevent="messageText += '\n'"
          type="text"
          class="message-input"
          v-model="messageText"
          placeholder="Введите сообщение..."
        ></textarea>
        <button
          class="send-button"
          :disabled="messageText === ''"
          @click="sendMessage"
        >
          Send
        </button>
      </div>
      <div class="message-list">
        <div class="message" v-for="message in messages" :key="message.id">
          <p class="message-text">{{ message }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      messageText: "",
      messages: [],
    };
  },
  methods: {
    sendMessage() {
      var ws = new WebSocket("ws://localhost:8080/");
      ws.onopen = () => {
        ws.send(this.messageText);
        this.messageText = "";
      };
      ws.onmessage = (evt) => {
        console.log(evt);
        this.messages.push(evt.data);
      };
    },
  },
};
</script>
<style lang="scss" scoped>
header {
  position: fixed;
  width: 100vw;
  background-color: #fff;
  top: 0;
  height: 50px;
  p {
    text-align: center;
    margin: 0 auto;
    width: 70%;
    max-width: 70%;
    padding: 10px;
  }
}
.app {
  position: fixed;
  width: 100%;
  height: 100vh;
  max-width: 100%;
  max-height: 100vh;
  padding: 60px 0 10px 0;
  margin: auto;
  background-color: gray;
}
.chat-window {
  position: relative;
  border-radius: 5px;
  background-color: #fff;
  width: 70%;
  height: 100%;
  margin: 0 auto;
  overflow-y: scroll;
  display: flex;
  flex-direction: column-reverse;
  gap: 20px;
  padding: 20px;

  textarea {
    resize: none;
    width: 100%;
    height: 50px;
  }
  .message-list {
    display: flex;
    flex-direction: column;
    gap: 10px;
    padding: 0 10px;
    .message {
      word-break: break-word;
      background-color: #27187510;
      padding: 10px 15px 12px 15px;
      border-radius: 8px;
    }
  }
  .input-group {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 20px;
    padding: 5px 10px;
  }
}

.chat-window {
  scrollbar-width: thin;
  scrollbar-color: #271875 #f2fdff00;
}

.chat-window::-webkit-scrollbar {
  width: 6px;
  width: 6px;
}
.chat-window::-webkit-scrollbar-track {
  border-radius: 5px;
  background-color: #f2fdff00;
  opacity: 0;
}

.chat-window::-webkit-scrollbar-track:hover {
  background-color: #b8c0c200;
  opacity: 0;
}

.chat-window::-webkit-scrollbar-track:active {
  background-color: #b8c0c200;
  opacity: 0;
}

.chat-window::-webkit-scrollbar-thumb {
  border-radius: 5px;
  background-color: #5181b870;
}

.chat-window::-webkit-scrollbar-thumb:hover {
  background-color: #5181b870;
}

.chat-window::-webkit-scrollbar-thumb:active {
  background-color: #5181b870;
}
</style>

