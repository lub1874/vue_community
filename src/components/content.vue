<template>
  <div class="warpper">
    <mu-appbar class="title" title="发表话题"></mu-appbar>
    <main>
      <mu-select-field class="text" v-model="list" :labelFocusClass="['label-foucs']" label="选择板块">
        <mu-menu-item v-for="(text, index) in lists" :key="index" :value="index" :title="text"></mu-menu-item>
      </mu-select-field>
      <mu-text-field class="text" v-model="title" label="标题" hintText="标题数字 10字以上"></mu-text-field>
      <mu-text-field v-model="content" class="text content" hintText="输入文本，支持markdown格式" multiLine :rows="5"
                     :underlineShow="false"></mu-text-field>
      <mu-raised-button class="demo-raised-button" @click="publishData" label="发表话题" icon="near me"
                        primary></mu-raised-button>
    </main>
    <mu-dialog :open="!accesstoken" title="提示:">
      请先登录

      <mu-flat-button slot="actions" primary to="/vuecommunitytest" label="取消"></mu-flat-button>
      <mu-flat-button slot="actions" primary to="/vuecommunitytest/login" label="登录"></mu-flat-button>
    </mu-dialog>
    <mu-dialog :open="publish" title="提示：">
      {{tips}}

      <mu-flat-button slot="actions" primary @click="close" label="确定"></mu-flat-button>
    </mu-dialog>
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
    padding: 1rem;
    display: flex;
    flex-direction: column;
  }

  .text {
    /*font-size: 24px;*/
    width: 100%;
    /*text-align: center;*/
  }

  .content {
    flex: 1;
    background-color: #f7f7f7;
    padding: 1rem;
    overflow-y: auto;
  }

  textarea {
    height: 10rem;
  }
</style>

<script>
  import bottomNavigation from "./common/BottomNavigation.vue"
  import axios from "axios"
  import marked from "marked"

  export default {
    components: {
      bottomNavigation
    },
    data () {
      return {
        msg: 'Welcome to Your Vue.js App',
        accesstoken: '',
        list: 0,
        lists: ['分享', '问答', '招聘'],
        title: '',
        tab: '',
        content: '',
        publish: false,
        tips: '请输入正确格式'
      }
    },
    created () {
      this.accesstoken = localStorage.getItem("accesstoken")
    },
    methods: {
      publishData () {
        if (this.list === 0) {
          this.tab = "share"
        } else if (this.list === 1) {
          this.tab = "ask"
        } else if (this.list === 2) {
          this.tab = "job"
        }

        let that = this
        that.content = marked(that.content)
        axios.post('https://www.vue-js.com/api/v1/topics', {
          accesstoken: that.accesstoken,
          title: that.title,
          tab: that.tab,
          content: that.content
        }).then((res) => {
          that.title = ''
          that.tab = ''
          that.tips = "发表成功"
          that.publish = true
          setTimeout(() => {
            that.publish = false
          }, 1500)
        }).catch((error) => {
          this.publish = false
        })
      },
      close () {
        this.publish = false
      }
    }
  }
</script>
