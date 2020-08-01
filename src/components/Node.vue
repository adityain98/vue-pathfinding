<template>
  <div
    :class="nodeClass"
    @mouseover="mouseOver()"
    @mousedown="toggleWall()"
  >
    <div
      v-if="nodeProperty.isRoad"
      class="road-node"
    >
    </div>
    <div
      v-else-if="nodeProperty.isVisited"
      class="visited"
    >
    </div>
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
      if (this.isMouseDown) {
        this.toggleWall()
      }
    },
    toggleWall () {
      if (!this.nodeProperty.isEnd && !this.nodeProperty.isStart) {
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
        // visited: visited,
        'start-node': this.nodeProperty.isStart,
        'end-node': this.nodeProperty.isEnd,
        // 'road-node': this.nodeProperty.isRoad,
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
  animation-name: animate-visited;
  animation-duration: 0.5s;
}

.start-node {
  background-color: blue;
}

.end-node {
  background-color: red;
}

.road-node {
  width: 20px;
  height: 20px;
  border: 1px solid rgb(175, 216, 248);
  background-color: yellow;
  animation-name: animate-road;
  animation-duration: 0.5s;
}

.wall {
  width: 20px;
  height: 20px;
  border: 1px solid rgb(175, 216, 248);
  background-color: blueviolet;
}

@keyframes animate-visited {
  from {
    background-color: teal;
  }
  to {
    background-color: greenyellow;
  }
}

@keyframes animate-road {
  from {
    background-color: brown;
  }
  to {
    background-color: yellow;
  }
}
</style>
