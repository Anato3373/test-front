<template>
  <div class="message-controls">
    <input
        type="text"
        v-model="newMessage"
        placeholder="Введите сообщение"
        @keyup.enter="send"
    >
    <button @click="send">Отправить</button>
  </div>
</template>

<script>
  import { ref } from 'vue'

  export default {
    name: 'MessageControls',
    emits: ['send-message'],
    setup(_props, { emit }) {
      const newMessage = ref('')

      const send = () => {
        if (newMessage.value.trim()) {
          emit('send-message', newMessage.value)
          newMessage.value = ''
        }
      }

      return {
        newMessage,
        send
      }
    }
  }
</script>

<style lang="scss" scoped>
  .message-controls {
    display: flex;
    gap: 10px;
  }

  .message-controls input {
    flex-grow: 1;
    padding: 8px;
    border: 1px solid #ddd;
    border-radius: 4px;
  }

  .message-controls button {
    padding: 15px 12px;
    max-width: 183px;
    width: 100%;
    background: linear-gradient(to right, #04694B, #11A87B);
    color: white;
    border: none;
    border-radius: 8px;
    cursor: pointer;
  }

  .message-controls button:hover {
    background-color: #45a049;
  }
</style>
