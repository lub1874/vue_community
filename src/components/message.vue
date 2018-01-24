<template>
  <div class="wrapper">
    <mu-appbar class="title" title="消息中心"></mu-appbar>
    <main>
      <mu-tabs class="activeTab" :value="activeTab" @change="handleTabChange">
        <mu-tab value="hasnot_read" title="未读消息"></mu-tab>
        <mu-tab value="has_read" title="已读消息"></mu-tab>
        <span v-if="count" class="count">{{count}}</span>
      </mu-tabs>

    </main>
  </div>
</template>

<style>

</style>

<script>

</script>
