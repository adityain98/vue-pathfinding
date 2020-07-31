<template>
  <div>
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
      }
    }
  },
  methods: {
    tableInit () {
      const arrTable = []
      let id = 0

      this.startNodePosition = {
        x: (this.height - 1) / 2,
        y: 6
      }
      this.endNodePosition = {
        x: (this.height - 1) / 2,
        y: this.width - 6
      }

      for (let x = 0; x < this.height; x++) {
        const arrTable2 = []

        for (let y = 0; y < this.width; y++) {
          id++

          if (x === this.startNodePosition.x && y === this.startNodePosition.y) {
            arrTable2.push({ ...this.startNode, id })
          }
          else if (x === this.endNodePosition.x && y === this.endNodePosition.y) {
            arrTable2.push({ ...this.endNode, id })
          }
          else if (this.isNearStartNode(x, y)) {
            //? if near node
            arrTable2.push({ ...this.baseNode, id, distance: 1 })
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
      if (x === this.endNodePosition.x - 1 && y === this.endNodePosition.y) {
        return true
      }
      else if (x === this.endNodePosition.x + 1 && y === this.endNodePosition.y) {
        return true
      }
      else if (x === this.endNodePosition.x && y === this.endNodePosition.y - 1) {
        return true
      }
      else if (x === this.endNodePosition.x && y === this.endNodePosition.y + 1) {
        return true
      }
      else return false
    }
  },
  created () {
    this.tableInit()
  }
}
</script>

<style>
.vertical-nodes {
  display: flex;
}
</style>
