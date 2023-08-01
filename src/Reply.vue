<template>
  <div class="sc-reply-box" :style="{backgroundColor: colors.replyMessage.bg}">
    <div class="sc-header--close-button" style="position: absolute;top: 10px;right: 10px;" @click="cancelReply">
      <img :src="closeIcon" alt="close-icon" width="14" height="14"/>
    </div>
    <div class="sc-message--text" style="margin-top: 6px; width: 70%;">
      Replying to message from {{ getParticipantName(replyMessage.author) }}
    </div>
    <div class="sc-message--reply">
      {{ truncateMessage() }}
    </div>
  </div>
</template>

<script>
import store, {mapState} from './store/'
import CloseIcon from './assets/close.svg'
export default {
  data() {
    return {
      closeIcon: CloseIcon
    }
  },
  props: {
    colors: {
      type: Object,
      required: true
    }
  },
  computed: mapState(['replyMessage','participants']),
  methods: {
    getParticipantName(participantId) {
      const participant = this.participants.find((p) => p.id === participantId);
      return participant ? participant.name : '';
    },
    cancelReply() {
      store.setState('replyMessage', null)
    },
    truncateMessage() {
      const truncatedMessage = this.replyMessage.data.text.substring(0, 45);
      return truncatedMessage.length < this.replyMessage.data.text.length ? truncatedMessage + " ..." : truncatedMessage;
    }
  },
}
</script>

<style>
.sc-reply-box {
  text-align: left;
  background: inherit;
  position: relative;
  padding-top: 6px;
}

.sc-header--close-button{
  cursor: pointer;
}
.sc-header--close-button:hover {
  box-shadow:0 2px 5px rgba(.2,.2,.5,.1)
}

.sc-message--reply {
  font-weight: 300;
  font-size: 12px;
  line-height: 1.2;
  padding: 5px 20px;
  white-space: pre-wrap;
  -webkit-font-smoothing: subpixel-antialiased;
  font-style: italic;
  opacity: .85;
}
</style>
