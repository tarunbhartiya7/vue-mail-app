<template>
  <div class="inbox-body">
    <app-messages :messages="importantMessages"></app-messages>
  </div>
</template>
<script>
  import Messages from './Messages.vue';

  export default {
    props: {
      data: {
        type: Object,
        required: true
      }
    },
    computed: {
      importantMessages() {
        return this.data.messages.filter(item => item.type === 'incoming' && item.isImportant && !item.isDeleted)
      }
    },
    components: {
      appMessages: Messages
    }
  }
</script>
<style scoped>
</style>
