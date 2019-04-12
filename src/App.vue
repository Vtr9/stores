<template>
  <div id="app">
    <Dito />
  </div>
</template>

<script>
import Dito from './components/Dito.vue'

export default {
  name: 'app',
  components: {
    Dito
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
  margin-top: 60px;
}
body {
  background-color: #DCDCDC;
}
</style>
