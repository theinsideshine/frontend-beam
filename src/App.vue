<template>
  <div id="app">
    <img alt="Udemm logo" src="./assets/udemm_logo.png">
    <img alt="Udemm logo"  height="200"    src="./assets/mecanismo_beam.png">
    <HelloWorld msg="Remote Lab: Frontend-beam V1.0.01"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
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
