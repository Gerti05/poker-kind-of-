<template>
  <div id="app">
    <PokerHands />
  </div>
</template>

<script>
import PokerHands from "./components/pokerHands"

export default {
  name: 'App',
  components: {
    PokerHands
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
