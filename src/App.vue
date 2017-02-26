<template>
  <div id="app">
    <div class="container">
      <div class="columns">
        <div class="column is-3">
          <div class="notification" :class="left ? 'is-success' : 'is-danger'" @click="left = !left">Tree (flat DOM)</div>
          <tree-flat v-if="left" :nodes="[tree]"></tree-flat>
        </div>
        <div class="column is-3">
          <div class="notification" :class="right ? 'is-success' : 'is-danger'" @click="right = !right">Tree (nested DOM)</div>
          <button class="button" disabled>Expand All</button>
          <div style="height: 600px; overflow: auto; position: relative">
            <tree-node v-if="right" :node="tree"></tree-node>
          </div>
        </div>
        <div class="column is-3">
          <div class="notification" :class="virtualized ? 'is-success' : 'is-danger'" @click="virtualized = !virtualized">Tree (flat DOM, virtualized)</div>
          <tree-flat v-if="virtualized" :nodes="[tree]" :virtualize="true"></tree-flat>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Hello from './components/Hello'
import TreeFlat from './components/TreeFlat'
import TreeNode from './components/TreeNode'

const randInt = (max) => Math.round(Math.random() * max)

let id = 0

const generateNode = (i, level) => {
  const children = generateNodes(randInt(2 * (6 - level)), level + 1)
  return {
    id: id++,
    text: `L${level} Node ${i}`,
    expanded: children.length ? Math.random() < 0.2 : false,
    children
  }
}

const generateNodes = (count, level) => [...Array(count)].map((x, i) => generateNode(i, level))

export default {
  name: 'app',
  data () {
    return {
      tree: generateNode(0, 0),
      left: true,
      right: true,
      virtualized: true
    }
  },
  components: {
    Hello,
    TreeFlat,
    TreeNode
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
