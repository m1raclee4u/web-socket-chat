<template>
  <div class="app">
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
        console.log(`Сообщение которое отправляется на сервер: ${this.messageText}`);
        this.messageText = "";
      };
      ws.onmessage = (evt) => {
        console.log(`Полученное сообщение с сервера: ${evt.data}`);
        this.messages.push(evt.data);
      };
    },
  },
};
</script>
<style lang="scss" scoped>
.app {
  position: fixed;
  width: 100%;
  height: 100vh;
  max-width: 100%;
  max-height: 100vh;
  padding: 60px 10px 10px 10px;
  margin: auto;
  background-color: gray;
}
.chat-window {
  position: relative;

  display: flex;
  flex-direction: column-reverse;
  gap: 20px;

  width: 100%;
  height: 100%;
  max-width: 931px;

  margin: 0 auto;

  border-radius: 5px;
  background-color: #fff;

  overflow-y: scroll;

  padding: 20px;
  height: 100%;

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
      background-color: #27187510;
      padding: 10px 15px 12px 15px;
      border-radius: 8px;
      p{
        word-break: break-word;

      }
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

