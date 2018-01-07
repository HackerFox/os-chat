<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-view/>
  </div>
</template>

<script>
import firebase from 'firebase'

export default {
  name: 'app',
  mounted: function () {
    // Initialize Firebase
    const config = {
      apiKey: "...",
      authDomain: "...",
      databaseURL: "...",
      projectId: "...",
    };
    firebase.initializeApp(config);
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
</style>
