<template>
  <HelloWorld/>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import {provide} from "vue";

let serverPort = '22111'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  setup() {
    let split = window.location.origin.split(":")
    let currentServerURL = split[0] + ":" + split[1] + (
        split[0].includes("https") ? "" : (":" + serverPort))
    console.log(currentServerURL)
    provide('defaultURL', currentServerURL)
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
