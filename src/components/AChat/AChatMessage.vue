<template>
  <div
    class="a-chat__message-container"
    :class="{ 'a-chat__message-container--right': sender.id === userId }"
  >
    <div
      class="a-chat__message"
      :class="{ 'a-chat__message--highlighted': sender.id === userId }"
    >
      <div
        v-if="showAvatar"
        class="a-chat__message-avatar"
        :class="{ 'a-chat__message-avatar--right': sender.id === userId }"
      >
        <slot name="avatar"></slot>
      </div>
      <div class="a-chat__message-card">
        <div class="a-chat__message-card-body">
          <div v-if="html" v-html="message" class="a-chat__message-text"></div>
          <div v-else v-text="message" class="a-chat__message-text"></div>
        </div>

        <div class="a-chat__message-card-header mt-2">
          <div :title="date" class="a-chat__timestamp">{{ time }}</div>
          <div class="a-chat__status">
            <v-icon v-if="status === 'sent'" size="15">mdi-clock-outline</v-icon>
            <v-icon
              v-else-if="status === 'rejected'"
              size="15"
              color="red"
              @click="$emit('resend')"
            >mdi-clock-outline</v-icon>
            <v-icon v-else size="15">mdi-check-all</v-icon>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment'

export default {
  mounted () {
    moment.locale(this.locale)
  },
  computed: {
    time () {
      return moment(this.timestamp).format('hh:mm A')
    },
    date () {
      return moment(this.timestamp).format('LLLL')
    }
  },
  props: {
    id: {
      type: null,
      required: true
    },
    message: {
      type: String,
      default: ''
    },
    timestamp: {
      type: Number,
      default: 0
    },
    status: {
      type: String,
      default: 'confirmed',
      validator: v => ['sent', 'confirmed', 'rejected'].includes(v)
    },
    userId: {
      type: String,
      default: ''
    },
    sender: {
      type: Object,
      required: true
    },
    showAvatar: {
      type: Boolean,
      default: true
    },
    locale: {
      type: String,
      default: 'en'
    },
    html: {
      type: Boolean,
      default: false
    }
  }
}
</script>