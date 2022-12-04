<template>
  <div id="app">
    <BaseDate />
  </div>
</template>

<script>
import BaseDate from './components/BaseDate.vue'

export default {
  name: 'App',
  components: {
    BaseDate
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
