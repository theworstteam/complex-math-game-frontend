<template>
  <div id="app">
      <Header />
      <router-view />
  </div>
</template>

<script>
import Header from './components/layout/Header';
export default {
  name: "app",
  components: {
    Header,
  }  
}
</script>


<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body{
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
}
</style>
