<template>
  <div class="wrapper">
    <mu-appbar class="title" title="消息中心"></mu-appbar>
    <main>
      <mu-tabs class="activeTab" :value="activeTab" @change="handleTabChange">
        <mu-tab value="hasnot_read" title="未读消息"></mu-tab>
        <mu-tab value="has_read" title="已读消息"></mu-tab>
        <span v-if="count" class="count">{{count}}</span>
      </mu-tabs>
      <!--未读消息列表-->
      <ul v-if="accesstoken" class="lists" v-show="activeTab === 'hasnot_read'">
        <router-link :to="{path='/vuecommunitytest/content', query: {id: item.topic.id}}" tag="li" class="list"
                     v-for="item in hasnot_read_messages" :key="item.id">
          <div class="user">
            <img :src="item.author.avatar_url" alt="user"/>
            <span>{{item.author.loginname}}</span>
          </div>
          <div class="content">
            <h2 v-html="markdownChange(item.reply.content)"></h2>
            <p>来自：《item.topic.title》</p>
          </div>
          <div class="timer">
            <span>{{item.reply.creat_at | timeago}}</span>
          </div>
        </router-link>
        <li v-if="!hasnot_read_messages.length">暂无消息</li>
      </ul>
      <!--已读消息-->
      <ul v-if="accesstoken" class="lists" v-show="activeTab === 'has_read'">
        <router-link :to="{path='/vuecommunitytest/content', query: {id: item.topic.id}}" tag="li" class="list"
                     v-for="item in has_read_messages" :key="item.id">
          <div class="user">
            <img :src="item.author.avatar_url" alt="user"/>
            <span>{{item.author.loginname}}</span>
          </div>
          <div class="content">
            <h2 v-html="markdownChange(item.reply.content)"></h2>
            <p>来自：《item.topic.title》</p>
          </div>
          <div class="timer">
            <span>{{item.reply.create_at | timeago}}</span>
          </div>
        </router-link>
        <li v-if="!has_read_messages.length">暂无消息</li>
      </ul>
      <p v-if="!accesstoken" class="tips">请先登录</p>
      <mu-raised-button class="btn" v-if="count" v-show="activeTab === 'hasnot_read'" @click="mark_all" label="标记全部已读"
                        icon="done_all" primary></mu-raised-button>
    </main>
    <bottom-navigation></bottom-navigation>
  </div>
</template>

<style scoped>
  .title {
    text-align: center;
    height: 5rem;
  }

  .wrapper {
    display: flex;
    flex-direction: column;
  }

  main {
    margin: 5rem 0;
    flex: 1;
    display: flex;
    flex-direction: column;
  }

  .activeTab {
    position: relative;
    border-top: 1px solid rgba(255, 255, 255, .1);
  }

  .count {
    position: absolute;
    left: 10%;
    top: 20%;
    background-color: #ff5252;
    width: 2rem;
    height: 2rem;
    line-height: 2rem;
    text-align: center;
    border-radius: 50%;
  }

  .lists {
    flex: 1;
    padding: 1rem;
    overflow-y: auto;
  }

  .btn {
    margin: 1rem;
  }

  .list {
    display: flex;
    justify-content: space-between;
    background-color: #f8f8f8;
    padding: 1rem;
    margin-bottom: 1rem;
  }

  .user {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 6rem;
  }

  .user > img {
    height: 4rem;
    width: 4rem;
    border-radius: 50%;
  }

  .user > span {
    text-align: center;
    max-width: 100%;
    word-wrap: break-word;
    word-break: normal;
  }

  .content {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
  }

  .content > h2 {
    font-weight: 700;
    font-size: 16px;
  }

  .content > p {
    color: #999;
    font-size: 12px;
  }

  .timer {
    margin-left: 1rem;
    width: 5rem;
    display: flex;
    align-items: center;
    justify-content: flex-end;
  }

  .tips {
    padding: 1rem;
  }
</style>

<script>
  import axios from 'axios'
  import marked from 'marked'
  import bottomNavigation from './common/BottomNavigation.vue'
  import timeago from 'timeago.js'

  export default {
    components: {
      bottomNavigation
    },
    data () {
      return {
        activeTab: 'hasnot_read',
        has_read_messages: [],
        hasnot_read_messages: [],
        accesstoken: '',
        count: null
      }
    },
    created () {
      this.accesstoken = localStorage.getItem("accesstoken")
      if (this.accesstoken) {
        this.getData()
      }
    },
    filters: {
      timeago(val) {
        let time = new Date(val)
        let thistime = timeago()
        return thistime.format(time, 'zh_CN')
      }
    },
    methods: {
      getData () {
        let that = this
        let token = that.accesstoken
        let url = 'https://www.vue-js.com/api/v1/messages?accesstoken=' + token

        axios.get(url).then((res) => {
          that.hasnot_read_messages = res.data.data.hasnot_read_messages
          that.has_read_messages = res.data.data.has_read_messages
        })

        axios.get('https://www.vue-js.com/api/v1/message/count?accesstoken=' + this.accesstoken).then(function (response) {
          //获取未读消息数量
          that.count = response.data.data
        })
      },
      mark_all () {
        let that = this
        axios.post('https://www.vue-js.com/api/v1/message/mark_all', {
          accesstoken: that.accesstoken
        })
          .then(function (response) {
            that.getData()
          })
      },
      handleTabChange (val) {
        this.activeTab = val
      },
      markdownChange (content) {
        return marked(content)
      }
    }
  }

</script>
