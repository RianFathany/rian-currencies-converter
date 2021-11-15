<template>
  <div id="app">
    <Converter msg="Welcome to Money Converter App"/>
  </div>
</template>

<script>
import Converter from './components/Converter.vue'

export default {
  name: 'App',
  components: {
    Converter
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding-top: 60px;
  background: rgb(171,126,227);
    background: -moz-linear-gradient(128deg, rgba(171,126,227,1) 0%, rgba(113,197,237,1) 31%, rgba(180,238,196,1) 100%);
    background: -webkit-linear-gradient(128deg, rgba(171,126,227,1) 0%, rgba(113,197,237,1) 31%, rgba(180,238,196,1) 100%);
    background: linear-gradient(128deg, rgba(171,126,227,1) 0%, rgba(113,197,237,1) 31%, rgba(180,238,196,1) 100%);
    filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#ab7ee3",endColorstr="#b4eec4",GradientType=1);
    background-repeat: no-repeat;
    height: 100vh;
    overflow: hidden;
}
</style>
