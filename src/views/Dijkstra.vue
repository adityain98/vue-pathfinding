<template>
  <div>
    <div id="pathfinding-wrapper">
      <div
        v-for="(vertical, i) in table"
        :key="`vertical-${i}`"
        class="vertical-nodes"
        @mousedown="mouseDown"
        @mouseup="mouseUp()"
      >
        <Node
          v-for="horizontal in vertical"
          :key="horizontal.id"
          :node-property="horizontal"
          :is-mouse-down="isMouseDown"
          @toggleWall="toggleWall"
          @moveNode="moveNode"
          @startDragNode="startDragNode"
          class="horizontal-nodes"
        />
      </div>
    </div>
    <div>{{errorMessage}}</div>
  </div>
</template>

<script>
import Node from '../components/Node'

export default {
  name: 'Pathfinding',
  components: {
    Node
  },
  data () {
    return {
      table: [],
      width: 60,
      height: 31,
      baseNode: {
        isWall: false,
        isStart: false,
        isVisited: false,
        isEnd: false,
        isRoad: false,
        distance: 10000
      },
      startNode: {
        isWall: false,
        isStart: true,
        isVisited: false,
        isEnd: false,
        isRoad: false,
        distance: 0
      },
      endNode: {
        isWall: false,
        isStart: false,
        isVisited: false,
        isEnd: true,
        isRoad: false,
        distance: 10000
      },
      startNodePosition: {
        x: 0,
        y: 0
      },
      endNodePosition: {
        x: 0,
        y: 0
      },
      shortestPath: [],
      isMouseDown: false,
      errorMessage: '',
      draggedNode: ''
    }
  },
  methods: {
    tableInit () {
      const arrTable = []
      let id = 0

      this.startNodePosition = {
        x: 15,
        y: (this.height - 1) / 2,
      }
      this.endNodePosition = {
        x: this.width - 10,
        y: (this.height - 1) / 2 + 4,
      }

      for (let y = 0; y < this.height; y++) {
        const arrTable2 = []

        for (let x = 0; x < this.width; x++) {
          id++

          if (y === this.startNodePosition.y && x === this.startNodePosition.x) {
            arrTable2.push({ ...this.startNode, id, x, y })
          }
          else if (y === this.endNodePosition.y && x === this.endNodePosition.x) {
            arrTable2.push({ ...this.endNode, id, x, y })
          }
          else if (this.isNearStartNode(x, y)) {
            //? if near node
            const newBaseNode = { ...this.baseNode }
            newBaseNode.distance = 1
            arrTable2.push({ ...newBaseNode, id, x, y })
          }
          else {
            arrTable2.push({ ...this.baseNode, id, x, y })
          }
        }
        arrTable.push([...arrTable2])
      }
      this.table = arrTable
    },
    isNearStartNode (x, y) {
      if (x === this.startNodePosition.x - 1 && y === this.startNodePosition.y) {
        return true
      }
      else if (x === this.startNodePosition.x + 1 && y === this.startNodePosition.y) {
        return true
      }
      else if (x === this.startNodePosition.x && y === this.startNodePosition.y - 1) {
        return true
      }
      else if (x === this.startNodePosition.x && y === this.startNodePosition.y + 1) {
        return true
      }
      else return false
    },
    setDistanceNearNode (x, y, distance) {
      if (
        y - 1 !== -1 &&
        !this.table[y - 1][x].isVisited &&
        !this.table[y - 1][x].isWall &&
        !this.table[y - 1][x].isStart
      ) {
        this.$set(this.table[y - 1][x], 'distance', distance + 1)
      }
      if (
        y + 1 !== this.height &&
        !this.table[y + 1][x].isVisited &&
        !this.table[y + 1][x].isWall &&
        !this.table[y + 1][x].isStart
      ) {
        this.$set(this.table[y + 1][x], 'distance', distance + 1)
      }
      if (
        x - 1 !== -1 &&
        !this.table[y][x - 1].isVisited &&
        !this.table[y][x - 1].isWall &&
        !this.table[y][x - 1].isStart
      ) {
        this.$set(this.table[y][x - 1], 'distance', distance + 1)
      }
      if (
        x + 1 !== this.width &&
        !this.table[y][x + 1].isVisited &&
        !this.table[y][x + 1].isWall &&
        !this.table[y][x + 1].isStart
      ) {
        this.$set(this.table[y][x + 1], 'distance', distance + 1)
      }
    },
    checkTheEnd (x, y) {
      if (
        this.table[y][x].isEnd
      ) {
        return true
      }
      return false
    },
    getShortestPath () {
      let x = this.endNodePosition.x
      let y = this.endNodePosition.y
      let currentDistance = this.table[y][x].distance
      let stateya123 = 0
      while (stateya123 < 1000) {
        if (
          y - 1 !== -1 &&
          !this.table[y - 1][x].isWall &&
          this.table[y - 1][x].distance == currentDistance - 1
        ) {
          this.shortestPath.unshift({
            x: x,
            y: y - 1
          })

          if (this.table[y - 1][x].isStart) {
            break
          }

          currentDistance = this.table[y - 1][x].distance
          y = y - 1

        }
        else if (
          y + 1 !== this.height &&
          !this.table[y + 1][x].isWall &&
          this.table[y + 1][x].distance == currentDistance - 1
        ) {
          this.shortestPath.unshift({
            x: x,
            y: y + 1
          })

          if (this.table[y + 1][x].isStart) {
            break
          }

          currentDistance = this.table[y + 1][x].distance
          y = y + 1

        }
        else if (
          x - 1 !== -1 &&
          !this.table[y][x - 1].isWall &&
          this.table[y][x - 1].distance == currentDistance - 1
        ) {
          this.shortestPath.unshift({
            x: x - 1,
            y: y
          })

          if (this.table[y][x - 1].isStart) {
            break
          }

          currentDistance = this.table[y][x - 1].distance
          x = x - 1

        }
        else if (
          x + 1 !== this.width &&
          !this.table[y][x + 1].isWall &&
          this.table[y][x + 1].distance == currentDistance - 1
        ) {
          this.shortestPath.unshift({
            x: x + 1,
            y: y
          })

          if (this.table[y][x - 1].isStart) {
            break
          }

          currentDistance = this.table[y][x + 1].distance
          x = x + 1

        }
        stateya123++
      }
      if (this.shortestPath.length > 0) {
        this.visualizePath()
      }
    },
    visualizePath () {
      let indexPath = 0
      if (this.shortestPath.length > 0) {
        const intervalVisualizePath = setInterval(() => {
          const x = this.shortestPath[indexPath].x
          const y = this.shortestPath[indexPath].y
          this.table[y][x].isRoad = true
          indexPath++
          if (indexPath === this.shortestPath.length) {
            clearInterval(intervalVisualizePath)
          }
        }, 40)
      }
    },
    startVisualizer () {
      let shortestDistance = 1
      let isPathNotFound = false
      const startIntervalVisualizer = setInterval(() => {
        let isFound = false
        for (let y = 0; y < this.height; y++) {
          for (let x = 0; x < this.width; x++) {
            if (
              !this.table[y][x].isWall &&
              !this.table[y][x].isStart &&
              !this.table[y][x].isVisited &&
              !isFound &&
              this.table[y][x].distance === shortestDistance
            ) {
              if (this.checkTheEnd(x, y)) {
                this.getShortestPath()
                clearInterval(startIntervalVisualizer)
                return true
              }
              this.table[y][x].isVisited = true
              this.setDistanceNearNode(x, y, this.table[y][x].distance)
              isFound = true
              isPathNotFound = false
            }
          }
        }
        if (!isFound) {
          if (isPathNotFound) {
            this.errorMessage = 'Path not found'
            clearInterval(startIntervalVisualizer)
          }
          shortestDistance++
          isPathNotFound = true
        }
      }, 5)
    },
    resetVisualizer () {
      this.shortestPath = []
      this.tableInit()
      this.errorMessage = ''
    },
    mouseDown (e) {
      if (e.which === 1) {
        this.isMouseDown = true
      }
    },
    mouseUp () {
      this.isMouseDown = false
    },
    toggleWall (x, y) {
      this.$set(this.table[y][x], 'isWall', !this.table[y][x].isWall)
    },
    startDragNode (node) {
      if (node.isEnd) {
        this.draggedNode = 'end'
      }
      else {
        this.draggedNode = 'start'
      }
      this.isMouseDown = false
    },
    moveNode (node) {
      if (this.draggedNode === 'end') {
        this.table.forEach(vertical => {
          vertical.forEach(horizontal => {
            horizontal.isEnd = false
          })
        })
        this.endNodePosition = {
          x: node.x,
          y: node.y
        }
        this.table[node.y][node.x].isEnd = true
      }
      else {
        this.table.forEach(vertical => {
          vertical.forEach(horizontal => {
            horizontal.isStart = false
          })
        })
        this.startNodePosition = {
          x: node.x,
          y: node.y
        }
        this.table[node.y][node.x].isStart = true
      }
      this.table[node.y][node.x].isWall = false
      this.resetVisited()
    },
    resetVisited () {
      this.shortestPath = []
      this.table.forEach((vertical, y) => {
        vertical.forEach((horizontal, x) => {
          horizontal.isVisited = false
          horizontal.isRoad = false
          if (!horizontal.isStart) {
            horizontal.distance = 10000
          }
          if (this.isNearStartNode(x, y)) {
            //? if near node
            const newBaseNode = { ...this.baseNode }
            horizontal.distance = 1
          }
        })
      })
    }
  },
  created () {
    this.tableInit()
  }
}
</script>

<style>
#pathfinding-wrapper {
  border: 1px solid rgb(175, 216, 248);
}

.vertical-nodes {
  display: flex;
}
</style>
