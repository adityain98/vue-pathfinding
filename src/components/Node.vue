<template>
  <div
    :class="nodeClass"
    @mouseover="mouseOver()"
    @mousedown="toggleWall"
    @dragenter="dragEnter()"
    @dragstart="dragStart()"
  >
    <div
      v-if="nodeProperty.isEnd"
      class="end-node"
    >
      <img
        src="../assets/end-moon.jpg"
        style="width: 100%"
      >
    </div>
    <div
      v-else-if="nodeProperty.isStart"
      class="start-node"
    >
      <img
        src="../assets/start-rocket.png"
        style="width: 100%"
      >
    </div>
    <div
      v-else-if="nodeProperty.isRoad"
      class="road-node"
    ></div>
    <div
      v-else-if="nodeProperty.isVisited"
      class="visited"
    ></div>
  </div>
</template>

<script>
export default {
  name: 'Node',
  props: {
    nodeProperty: Object,
    isMouseDown: Boolean,
    currentDraggedCoordinate: Object
  },
  methods: {
    mouseOver () {
      if (this.isMouseDown) {
        if (!this.nodeProperty.isEnd && !this.nodeProperty.isStart) {
          this.$emit('toggleWall', this.nodeProperty.x, this.nodeProperty.y)
        }
      }
    },
    toggleWall (e) {
      if (!this.nodeProperty.isEnd && !this.nodeProperty.isStart && e.which === 1) {
        this.$emit('toggleWall', this.nodeProperty.x, this.nodeProperty.y)
      }
    },
    dragEnter () {
      this.$emit('moveNode', this.nodeProperty)
    },
    dragStart () {
      this.$emit('startDragNode', this.nodeProperty)
    }
  },
  computed: {
    nodeClass () {
      const unvisited = !this.nodeProperty.isWall && !this.nodeProperty.isVisited
      const visited = !this.nodeProperty.isWall && this.nodeProperty.isVisited

      return {
        unvisited: unvisited,
        // visited: visited,
        // 'start-node': this.nodeProperty.isStart,
        // 'end-node': this.nodeProperty.isEnd,
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
  width: 20px;
  height: 20px;
}

.end-node {
  width: 20px;
  height: 20px;
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
  0% {
    background-color: white;
  }
  25% {
    background-color: teal;
  }
  100% {
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
