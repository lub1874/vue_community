<template>
  <div class="hello">
    <h1 @click="getData">{{ msg }}</h1>
  </div>
</template>

<style scoped>
  h1, h2 {
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }
</style>

<script>
  import axios from 'axios'
  import timeago from 'timeago.js'
  export default {
    name: "hello",
    data () {
      return {
        msg: 'Welcome to You Vue.js App'
      }
    },
    methods: {
      getData() {
        let that = this
        axios.get('http://www.vue-js.com/api/v1/topics?tab=all')
          .then(function (response) {
            console.log(response.data)
            let time = new Date(response.data.data[4].last_reply_at)
            console.log(time)
            let thistime = timeago()
            // thistime.format('2017-04-29')
            console.log(thistime.format(time, 'zh_CN'))
          })

        //用户详情
        axios.get('http://www.vue-js.com/api/v1/user/bailicangdu')
          .then(function (response) {
            // console.log(response.data)
          })
        axios.get('http://www.vue-js.com/api/v1/topic/5904878e4d2e582056ec6b3e')
          .then(function (response) {
            // console.log(response.data)
          })
      }
    }
  }
</script>
