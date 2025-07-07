<template>
  <div class="message-display">
    <div
        v-if="currentMessage"
        class="message-container"
    >
      <span
          class="close-btn"
          @click="removeCurrentMessage"
      >
        ×
      </span>
      <div class="message-content">{{ currentMessage.text }}</div>
    </div>
  </div>
</template>

<script>
  import {
    ref,
    watch,
    onMounted,
    onBeforeUnmount
  } from 'vue'

  export default {
    name: 'MessageDisplay',
    props: {
      messages: {
        type: Array,
        required: true
      }
    },
    emits: ['remove-message'],
    setup(props, { emit }) {
      const currentMessage = ref(null)
      const currentIndex = ref(0)
      const interval = ref(null)

      const showNextMessage = () => {
        if (props.messages.length === 0) {
          currentMessage.value = null
          return
        }

        currentIndex.value = (currentIndex.value + 1) % props.messages.length
        currentMessage.value = props.messages[currentIndex.value]
      }

      const removeCurrentMessage = () => {
        emit('remove-message', currentMessage.value.id)
      }

      watch(() => props.messages, (newMessages) => {
        if (newMessages.length > 0) {
          currentIndex.value = 0
          currentMessage.value = newMessages[0]
        } else {
          currentMessage.value = null
        }
      }, { immediate: true })

      onMounted(() => {
        interval.value = setInterval(showNextMessage, 3000)
      })

      onBeforeUnmount(() => {
        clearInterval(interval.value)
      })

      return {
        currentMessage,
        removeCurrentMessage
      }
    }
  }
</script>

<style lang="scss" scoped>
  .message-display {
    margin-bottom: 20px;
    background-color: #E4F0ED;
    border-radius: 6px;
    position: relative;
    padding: 20px 16px;
  }

  .message-container {
    padding-right: 30px;
    display: flex;
    align-items: center;
    gap: 16px;
  }

  .close-btn {
    cursor: pointer;
    font-size: 24px;
    color: #2C674D;
  }

  .close-btn:hover {
    color: #333;
  }

  .message-content {
    font-size: 18px;
  }
</style>
