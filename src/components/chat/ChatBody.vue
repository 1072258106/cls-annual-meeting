<template>
  <div class="body-wrapper" id="body-wrapper">
    <template v-for="msgObj in msgs">
      <template v-if="msgObj.login">
        <system-msg :msg="msgObj.user.username"></system-msg>
      </template>
      <template v-if="!msgObj.login&&!msgObj.logout">
        <other-msg v-if="msgObj.UUID!=chat.UUID" :name="msgObj.username" :msg="msgObj.msg" :color="msgObj.color" :photo="msgObj.photo"></other-msg>
      <self-msg v-if="msgObj.UUID==chat.UUID" :msg="msgObj.msg" :color="msgObj.color" :photo="msgObj.photo" ></self-msg>
      </template>
    </template>
  </div>

</template>

<script>
  import OtherMsg from './OtherMsg'
  import SelfMsg from './SelfMsg'
  import SystemMsg from './SystemMsg'
  import CHAT from '../../api/chat'

  export default {
    name: 'ChatBody',
    data () {
      return {
        chat : CHAT,
        msgs : CHAT.msgArr
      }
    },
    ready () {
      if (!localStorage.getItem('name')) {
        this.$router.go('/login')
      }
    },
    watch : {
      msgs () {
        setTimeout(function () {
          document.getElementById('body-wrapper').scrollTop = document.getElementById('body-wrapper').scrollHeight
        }, 20)
      }
    },
    components: {
      OtherMsg,
      SelfMsg,
      SystemMsg
    },
    methods: {
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.body-wrapper{
  background-color: #f6f6f6;
  height: calc(100% - 180px);
  overflow-y: scroll;
  overflow-x: hidden;
  background: url(../../assets/bg_chat.jpg) no-repeat;
  background-size: 100%;
  /*padding-bottom: 120px;*/
}
</style>
