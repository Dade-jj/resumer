<template>
  <div id="topbar">
    <div class="wrapper">
      <span class="logo">Resumer</span>
      <div class="actions">
        <div v-if="logined" class="userActions">
          <span class="welcome">你好，{{user.username}}</span>
          <el-button class="button" href="#" @click.prevent="signOut">登出</el-button>
        </div>
        <div v-else class="userActions">
          <el-button type="primary" href="#" class="button primary" @click.prevent="signUpDialogVisible=true">注册</el-button>
          <el-button class="button" @click.prevent="signInDialogVisible = true" href="#">登录</el-button>
        </div>
        <el-button type="primary" class="button primary">保存</el-button>
        <el-button class="button">预览</el-button>
      </div>
    </div>
    <MyDialog title="注册" :visible="signUpDialogVisible" @close="signUpDialogVisible = false">
      <SignUpForm @success="signIn($event)"/>
    </MyDialog>
    <MyDialog title="登录" :visible="signInDialogVisible" @close="signInDialogVisible = true">
      <SignInForm @success="signIn($event)" />
    </MyDialog>
  </div>
</template>

<script>
import MyDialog from './MyDialog'
import SignUpForm from './SignUpForm'
import SignInForm from './SignInForm'
import AV from '../lib/leancloud'

export default {
  name: 'Topbar',
  data () {
    return {
      signUpDialogVisible: false,
      signInDialogVisible: false
    }
  },
  components: {
    MyDialog,SignUpForm,SignInForm
  },
  computed: {
    user () {
      return this.$store.state.user
    }
  },
  methods: {
    signOut () {
      AV.User.logOut()
      this.$store.commit('removeUser')
    },
    signIn (user) {
      this.signUpDialogVisible = false
      this.signInDialogVisible = false
      this.$store.commit('setUser', user)
    },
    logined () {
      return this.user.id;
    }
  }
}
</script>

<style scoped lang="scss">
#topbar {
  background-color: #ffffff;
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.25);
  .wrapper {
    min-width: 1024px;
    max-width: 1440px;
    margin: 0 auto;
    height: 64px;
  }
  .wrapper {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 16px;
  }
  .logo {
    font-size: 24px;
    color: #000;
  }
}

  .actions {
    display: flex;
    .userActions {
      margin-right: 3em;
      .welcome {
        margin-right: .5em;
      }
     }
  }

</style>
