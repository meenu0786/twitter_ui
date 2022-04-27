<template>
  <div id="app">
    <Twitter />
  </div>
</template>

<script>
import Twitter from './components/Twitter.vue'

export default {
  name: 'App',
  components: {
    Twitter
  }
}
</script>

<style>
#app {
  margin: 0 20px;
}
</style>
