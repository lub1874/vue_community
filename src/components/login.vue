<template>
  <div class="wrapper">
    <mu-appbar title="登录" class="title"></mu-appbar>
    <main>
      <mu-text-field v-model="val" label="Access Token" :errorText="error" labelFloat></mu-text-field>
      <mu-raised-button @click="login" label="登录" class="demo-raised-button" primary></mu-raised-button>
    </main>
    <bottom-navigation></bottom-navigation>
  </div>
</template>

<style scoped>
  .wrapper {
    display: flex;
    flex-direction: column;
  }

  .title {
    text-align: center;
    height: 5rem;
  }

  main {
    flex: 1;
    margin: 5rem 0;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .demo-raised-button {
    margin-top: 2rem;
    width: 22rem;
  }
</style>

<script>
  import axios from 'axios'
  import bottomNavigation from "./common/BottomNavigation.vue"
  export default {
    components: {
      bottomNavigation
    },
    data () {
      return {
        val: '',
        error: ''
      }
    },
    methods: {
      login () {
        let that = this
        axios.post('https://www.vue-js.com/api/v1/accesstoken', {
          accesstoken: that.val
        }).then(function (res) {
          that.error = ''
          localStorage.setItem("accesstoken", that.val)
          localStorage.setItem("user_id", res.data.id)
          localStorage.setItem("login_name", res.data.loginname)
          that.$router.push({
            path: '/vuecommunitytest/personal'
          })
        }).catch(function (error) {
          console.log(error)
          that.error = "输入错误，请重新输入"
          console.log(that.$router.matched)
          that.$router.matched[0].meta = {
            requiresAuth: true
          }
        })
      }
    }
  }
</script>
