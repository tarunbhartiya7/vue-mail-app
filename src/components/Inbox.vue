<template>
  <div class="inbox-body">
    <div class="mail-option">
      <div class="btn-group">
          <a href="#" class="btn" @click="refresh">
              <i class="fa fa-refresh"></i>&nbsp; Refresh
          </a>
      </div>
    </div>
    <app-messages :messages="incomingMessages"></app-messages>
  </div>
</template>
<script>
  import Messages from './Messages.vue';
  import { eventBus } from '../main';

  export default {
    props: {
      data: {
          type: Object,
          required: true
      }
    },
    methods: {
      refresh() {
          eventBus.$emit('refreshMessages');
      }
    },
    computed: {
      incomingMessages() {
        return this.data.messages.filter(item => item.type === 'incoming' && !item.isDeleted)
      }
    },
    components: {
      appMessages: Messages
    }
  }
</script>
<style scoped>
</style>
