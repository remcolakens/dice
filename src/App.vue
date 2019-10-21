<template>
  <div id="app">
    <Dice magic-number="1" />
  </div>
</template>

<script>
import Dice from './components/Dice.vue'

export default {
  name: 'app',
  components: {
    Dice
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  height: 100vh;
}
</style>
