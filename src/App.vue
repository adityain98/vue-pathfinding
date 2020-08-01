<template>
  <div id="app">
    <div>
      <Dijkstra ref="dijkstra" />
    </div>
    <button @click="startVisualizer()">start</button>
    <button @click="resetVisualizer()">reset</button>
    <button @click="resetVisited()">start again</button>
  </div>
</template>

<script>
import Dijkstra from './views/Dijkstra'

export default {
  name: 'App',
  components: {
    Dijkstra
  },
  data () {
    return {
      algorithm: 'dijkstra'
    }
  },
  methods: {
    startVisualizer () {
      this.$refs[this.algorithm].startVisualizer()
    },
    resetVisualizer () {
      this.$refs[this.algorithm].resetVisualizer()
    },
    resetVisited () {
      this.$refs[this.algorithm].resetVisited()
    }
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
