<template>
  <div id="app">
    <Swap msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import Swap from "./components/Swap";

export default {
  name: 'App',
  components: {
    Swap
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
