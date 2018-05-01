<template>
    <aside class="sm-side">
        <div class="user-head">
            <img src="../assets/images/profile.jpg">

            <div class="user-name">
                <h5>Bo Andersen</h5>
                <span class="email-address">info@codingexplained.com</span>
            </div>
        </div>

        <div class="compose-wrapper">
          <app-compose></app-compose>
        </div>

        <ul class="inbox-nav">
            <li :class="{ active: activeView === 'app-inbox' }">
                <a href="#" @click="navigate('app-inbox', 'Inbox')">
                    <i class="fa fa-inbox"></i>Inbox <span class="label label-danger pull-right">{{ unreadMessages.length }}</span>
                </a>
            </li>

            <li :class="{ active: activeView === 'app-sent' }">
                <a href="#" @click="navigate('app-sent', 'Sent')">
                    <i class="fa fa-envelope-o"></i>Sent <span class="label label-default pull-right">{{ sentMessages.length }}</span>
                </a>
            </li>

            <li :class="{ active: activeView === 'app-important' }">
                <a href="#" @click="navigate('app-important', 'Important')">
                    <i class="fa fa-bookmark-o"></i>Important <span class="label label-warning pull-right">{{ importantMessages.length }}</span>
                </a>
            </li>

            <li :class="{ active: activeView === 'app-trash' }">
                <a href="#" @click="navigate('app-trash', 'Trash')">
                    <i class=" fa fa-trash-o"></i>Trash <span class="label label-default pull-right">{{ trashedMessages.length }}</span>
                </a>
            </li>
        </ul>
    </aside>
</template>
<script>
  import { eventBus } from '../main';
  import Compose from './Compose.vue';

  export default {
    props: {
      messages: {
        type: Array,
        required: true
      }
    },
    created() {
      //do something after creating vue instance
      eventBus.$on('changeView', data => this.activeView = data.tag);
      this.unreadMessages;
    },
    data() {
      return {
        activeView: 'app-inbox'
      }
    },
    components: {
      appCompose: Compose
    },
    methods: {
      navigate(newView, title) {
        eventBus.$emit('changeView', {
          tag: newView,
          title
        })
      }
    },
    computed: {
      unreadMessages() {
        return this.messages.filter(item => item.type === 'incoming' && !item.isRead && !item.isDeleted)
      },
      sentMessages() {
        return this.messages.filter(item => item.type === 'outgoing' && !item.isDeleted)
      },
      importantMessages() {
        return this.messages.filter(item => item.type === 'incoming' && item.isImportant && !item.isDeleted)
      },
      trashedMessages() {
        return this.messages.filter(item => item.isDeleted)
      }
    }
  }
</script>
<style scoped>
</style>
