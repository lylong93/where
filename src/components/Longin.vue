<template>
  <div class="longin-wrapper">
    <div class="login-input">
      <span>账号</span>
      <l-input placeholder="输入账号" v-model="state.name"></l-input>
      <span class="msg">{{msg}}</span>
    </div>
    <div>
      <span>密码</span>
      <l-input placeholder="输入密码" type="password" v-model="state.pwd" />
    </div>
    <div class="login-button">
      <l-button @click="login" :disabled="disabled">登录</l-button>
    </div>
  </div>
</template>
<script>
import { mapMutations } from 'vuex'
import button from '../base/button'
import input from '../base/input'
import api from '../api'
export default {
  data() {
    return {
      state: {
        name: '',
        pwd: ''
      },
      msg: ''
    }
  },
  methods: {
    ...mapMutations([
      'change'
    ]),
    login() {
      api.Login(this.state)
        .then((data) => {
          return data.data
        })
        .then((data) => {
          if (data.err === 0) {
            this.change(data.data.name)
            this.$router.push('Ok')
          } else {
            this.msg = data.msg
          }
        })
    }
  },
  computed: {
    disabled() {
      if (this.state.name && this.state.pwd) {
        return false
      } else {
        this.msg = ''
        return true
      }
    }
  },
  components: {
    LButton: button,
    LInput: input
  }
}

</script>
<style lang="scss">
.longin-wrapper {
  height: 70%;
  margin: 0 auto;
  width: 80%;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  .login-input {
    position: relative;
    .msg {
      position: absolute;
      left: 50px;
      top: 50px;
      color: red;
    }
  }
  .login-button {
    position: absolute;
    width: 80%;
    bottom: 5%;
    left: 10%;
  }
}

</style>
