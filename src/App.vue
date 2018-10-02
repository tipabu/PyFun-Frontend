<template>
  <div id="app">
    <navigator/>
    <div class="container">
      <router-view class="inner sm-4 sm-offset-4"/>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }
  .inner {
    padding-top: 60px;
  }
</style>
