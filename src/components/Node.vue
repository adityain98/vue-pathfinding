<template>
  <div :class="nodeClass">
  </div>
</template>

<script>
export default {
  name: 'Node',
  props: {
    nodeProperty: Object
  },
  computed: {
    nodeClass () {
      const unvisited = !this.nodeProperty.isWall && !this.nodeProperty.isVisited
      const visited = !this.nodeProperty.isWall && this.nodeProperty.isVisited

      return {
        unvisited: unvisited,
        visited: visited,
        'start-node': this.nodeProperty.isStart,
        'end-node': this.nodeProperty.isEnd,
      }
    }
  }
}
</script>

<style>
.unvisited {
  width: 20px;
  height: 20px;
  border: 1px solid rgb(175, 216, 248);
}

.start-node {
  background-color: blue;
}

.end-node {
  background-color: red;
}
</style>
