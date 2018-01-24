<template>
  <div class="warpper">
    <isheader class="header"></isheader>
    <tabs class="tab"></tabs>
    <bottom-navigation></bottom-navigation>
  </div>
</template>

<style scoped>
  .wrapper {
    display: flex;
    flex-direction: column;
    height: 100vh;
  }

  .tab {
    flex: 1;
    margin: 9rem 0 5rem 0;
  }

  .header {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1;
  }
</style>

<script>
  import isheader from './common/header.vue'
  import bottomNavigation from './common/BottomNavigation.vue'
  import tabs from './common/tabs.vue'
  import axios from 'axios'

  export default {
    components: {
      isheader,
      bottomNavigation,
      tabs
    },
    data () {
      return {
        msg: 'Welcome to Your Vue.js App'
      }
    }
  }
</script>
