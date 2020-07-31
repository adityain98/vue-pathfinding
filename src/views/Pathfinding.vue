<template>
  <div>
    <div id="pathfinding-wrapper">
      <div
        v-for="(vertical, i) in table"
        :key="`vertical-${i}`"
        class="vertical-nodes"
      >
        <Node
          v-for="horizontal in vertical"
          :key="horizontal.id"
          :node-property="horizontal"
          class="horizontal-nodes"
        />
      </div>
    </div>
    <button @click="startVisualizer()">start</button>
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
      width: 70,
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
      shortestPath: []
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
        x: this.width - 40,
        y: (this.height - 1) / 2,
      }

      for (let y = 0; y < this.height; y++) {
        const arrTable2 = []

        for (let x = 0; x < this.width; x++) {
          id++

          if (y === this.startNodePosition.y && x === this.startNodePosition.x) {
            arrTable2.push({ ...this.startNode, id })
          }
          else if (y === this.endNodePosition.y && x === this.endNodePosition.x) {
            arrTable2.push({ ...this.endNode, id })
          }
          else if (this.isNearStartNode(x, y)) {
            //? if near node
            const newBaseNode = { ...this.baseNode }
            newBaseNode.distance = 1
            arrTable2.push({ ...newBaseNode, id })
          }
          else {
            arrTable2.push({ ...this.baseNode, id })
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
      const x = this.endNodePosition.x
      const y = this.endNodePosition.y
      console.log(this.table[y][x])
    },
    startVisualizer () {
      let shortestDistance = 1
      const intervalVisualizaton = setInterval(() => {
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
                clearInterval(intervalVisualizaton)
                this.getShortestPath()
                break
              }
              this.table[y][x].isVisited = true
              this.setDistanceNearNode(x, y, this.table[y][x].distance)
              isFound = true
            }
          }
        }
        if (!isFound) {
          shortestDistance++
        }
      }, 5)
    },
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
