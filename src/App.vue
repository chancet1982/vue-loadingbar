<template>
  <div id="app">
	<loading-bar></loading-bar>
    <img src="./assets/logo.png">
	<h1>testing loading bar</h1>
  </div>
</template>

<script>
import LoadingBar from './components/LoadingBar'

export default {
  name: 'app',
  components: {
    LoadingBar,
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
