<template>

  <div id="app" class="wrapper">
    <keep-alive>
      <router-view></router-view>
    </keep-alive>

    <main-tab-bar></main-tab-bar>
  <!--  <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>-->
  </div>
</template>

<script>
/*import HelloWorld from './components/HelloWorld.vue'*/

import MainTabBar from './components/content/mainTabbar/MainTabBar'
export default {
  name: 'App',
  components: {
   // HelloWorld
    MainTabBar
  }
}
</script>

<style>
  @import "./assets/css/base.css";

/*#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}*/
</style>
