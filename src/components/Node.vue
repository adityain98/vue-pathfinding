<template>
  <div :class="nodeClass">
    <!-- <div v-if="nodeProperty.distance !== 10000">{{nodeProperty.distance}}</div> -->
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
        'road-node': this.nodeProperty.isRoad
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

.visited {
  width: 20px;
  height: 20px;
  border: 1px solid rgb(175, 216, 248);
  background-color: greenyellow;
}

.start-node {
  background-color: blue;
}

.end-node {
  background-color: red;
}

.road-node {
  background-color: yellow;
}
</style>
