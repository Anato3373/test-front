<template>
  <div class="messages">
    <div class="messages-wrapper">
      <MessageDisplay
          :messages="activeMessages"
          @remove-message="removeMessage"
      />
      <MessageList
          :messages="allMessages"
          :removed-messages="removedMessages"
      />
      <MessageControl
          @send-message="sendMessage"
      />
    </div>
  </div>
</template>

<script>
  import { ref, computed } from 'vue'

  import MessageDisplay from '@/components/MessageDisplay.vue'
  import MessageList from '@/components//MessageList.vue'
  import MessageControl from '@/components/MessageControl.vue'

  export default {
    name: 'App',
    components: {
      MessageDisplay,
      MessageList,
      MessageControl
    },
    setup() {
      const allMessages = ref([])
      const removedMessages = ref([])
      const nextId = ref(1)

      const activeMessages = computed(() =>
          allMessages.value.filter(
              message => !removedMessages.value.includes(message.id)
          )
      )

      const sendMessage = (text) => {
        allMessages.value.push({
          id: nextId.value++,
          text: text,
          createdAt: Date.now()
        })
      }

      const removeMessage = (id) => {
        if (!removedMessages.value.includes(id)) {
          removedMessages.value.push(id)
        }
      }

      // Инициализация с тестовыми сообщениями
      const initialMessages = [
        'Сообщение 1',
        'Сообщение 2',
        'Сообщение 3',
        'Сообщение 4',
        'Сообщение 5'
      ]

      initialMessages.forEach(msg => {
        sendMessage(msg)
      })

      return {
        allMessages,
        removedMessages,
        activeMessages,
        sendMessage,
        removeMessage
      }
    }
  }
</script>

<style lang="scss" scoped>
  .messages {
    height: 100vh;
    padding: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .messages-wrapper {
    max-width: 665px;
    width: 100%;
    border-radius: 8px;
    padding: 20px;
    box-shadow: 0 4px 20px #2123251F;
  }
</style>
