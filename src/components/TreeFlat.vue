<template lang="html">
  <div class="tree">
    <button class="button" @click="expandAll">Expand All</button>
    <transition-group name="node">
      <flat-node class="node" v-for="x in flatNodes" :key="x.node.id" :node="x.node" :level="x.level"></flat-node>
    </transition-group>
  </div>
</template>

<script>
import FlatNode from './FlatNode'

const apply = (nodes, f) => {
  for (const node of nodes) {
    f(node)
    if (node.children) {
      apply(node.children, f)
    }
  }
}

const flattenVisible = (nodes, level = 0) => {
  const flattened = []
  for (const node of nodes) {
    flattened.push({node, level})
    if (node.children && node.expanded) {
      flattened.push(...flattenVisible(node.children, level + 1))
    }
  }
  return flattened
}

export default {
  props: ['nodes', 'cluster'],
  data () {
    return {
      roots: []
    }
  },
  computed: {
    flatNodes () { return flattenVisible(this.roots) }
  },
  components: {
    FlatNode
  },
  methods: {
    expandAll () {
      const nodes = this.roots
      this.roots = []
      apply(nodes, n => { n.expanded = true })
      this.roots = nodes
    }
  },
  watch: {
    nodes (value) { this.roots = value }
  },
  created () {
    this.roots = this.nodes
  }
}
</script>

<style lang="css" scoped>

.node-move {
  transition: transform 0.3s;
}

.node-enter-active {
  transition: all 0.4s;
}

.node-leave-active {
  position: absolute;
  transition: all 0.2s;
}

.node-enter, .node-leave-to {
  transform: translateX(-1rem);
  opacity: 0;
}

</style>
