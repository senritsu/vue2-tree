<template>
  <div id="app">
    <div class="container">
      <div class="columns">
        <div class="column is-3">
          <div class="notification is-info">Tree (flat DOM)</div>
          <tree-flat :nodes="[tree]"></tree-flat>
        </div>
        <div class="column is-3">
          <div class="notification is-info">Tree (nested DOM)</div>
          <button class="button" disabled>Expand All</button>
          <tree-node :node="tree"></tree-node>
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
      tree: generateNode(0, 0)
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
