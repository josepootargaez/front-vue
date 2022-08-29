<template>
<div class="container">
  <img alt="Vue logo"  style="width:10%;" src="./assets/user.png">
  <templateindex msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import templateindex from './components/templateindex.vue'
import "bootstrap/dist/css/bootstrap.css";
export default {
  name: 'App',
  components: {
    templateindex
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
