<template>
  <div id="app">
    <Game></Game>
  </div>
</template>

<script>
import Game from './components/game'

export default {
  name: 'App',
  components: {
    Game
  }
}
</script>

<style>
html, body, #app, .game{
  width: 100%;
  height: 100%;
  margin: 0;
}
</style>
