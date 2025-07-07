<template>
  <div class="message-list">
    <div
        v-for="message in messages"
        :key="message.id"
        class="message-item"
    >
      <span v-if="removedMessages.includes(message.id)" class="removed-marker">×</span>
      <span class="message-text">{{ message.text }}</span>
      <span class="message-time">{{ formatTime(message.createdAt) }}</span>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'MessageList',
    props: {
      messages: {
        type: Array,
        required: true
      },
      removedMessages: {
        type: Array,
        required: true
      }
    },
    setup() {
      const formatTime = (timestamp) => {
        return new Date(timestamp).toLocaleString()
      }

      return {
        formatTime
      }
    }
  }
</script>

<style lang="scss" scoped>
  .message-list {
    padding: 20px 30px;
    margin-bottom: 20px;
    background-color: #F6F8FA;
    border-radius: 6px;
  }

  .message-item {
    padding: 8px 0;
    display: flex;
    align-items: center;
    position: relative;
  }

  .message-item:last-child {
    border-bottom: none;
  }

  .message-text {
    flex-grow: 1;
  }

  .message-time {
    color: #999;
    font-size: 0.9em;
    margin-left: 15px;
  }

  .removed-marker {
    color: #000;
    font-weight: bold;
    position: absolute;
    top: 25%;
    left: -15px;
  }
</style>
