<template>
  <div id="app">
    <echart/>
  </div>
</template>

<script>
import Echart from './components/Echart'

export default {
  name: 'App',
  components: {
    Echart
  }
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
</style>
