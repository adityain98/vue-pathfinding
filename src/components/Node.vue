<template>
  <div
    :class="nodeClass"
    @mouseover="mouseOver()"
  >
  </div>
</template>

<script>
export default {
  name: 'Node',
  props: {
    nodeProperty: Object,
    isMouseDown: Boolean
  },
  methods: {
    mouseOver () {
      if (this.isMouseDown && !this.nodeProperty.isEnd && !this.nodeProperty.isStart) {
        console.log(this.nodeProperty.isStart)
        this.$emit('toggleWall', this.nodeProperty.x, this.nodeProperty.y)
      }
    }
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
        'road-node': this.nodeProperty.isRoad,
        'wall': this.nodeProperty.isWall
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

.wall {
  width: 20px;
  height: 20px;
  border: 1px solid rgb(175, 216, 248);
  background-color: blueviolet;
}
</style>
