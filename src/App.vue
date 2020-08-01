<template>
  <div id="app">
    <Pathfinding />
  </div>
</template>

<script>
import Pathfinding from './views/Pathfinding'

export default {
  name: 'App',
  components: {
    Pathfinding
  }
}
</script>

<style>
::-moz-selection {
  background-color: transparent;
  color: #000;
}

::selection {
  background-color: transparent;
  color: #000;
}
</style>
