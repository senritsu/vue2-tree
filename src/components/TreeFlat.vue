<template lang="html">
  <div class="tree">
    <button class="button" @click="expandAll">Expand All</button>
    <div style="height: 600px; overflow: auto; position: relative" ref="container">
      <div :style="{height: `${paddingTop}px`}"></div>
      <transition-group name="node" tag="div">
        <flat-node class="node" v-for="x in (virtualize ? visibleChunk : flatNodes)" :key="x.node.id" :node="x.node" :level="x.level" :index="x.index" @expanded="updateVirtualization"></flat-node>
      </transition-group>
      <div :style="{height: `${paddingBottom}px`}"></div>
    </div>
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
  props: ['nodes', 'virtualize'],
  data () {
    return {
      roots: [],
      height: 0,
      firstVisibleIndex: 0,
      paddingTop: 0,
      paddingBottom: 0,
      itemsPerChunk: 0,
      itemHeight: 0
    }
  },
  computed: {
    flatNodes () {
      return flattenVisible(this.roots)
    },
    visibleChunk () {
      return this.flatNodes.slice(this.firstVisibleIndex, this.firstVisibleIndex + this.itemsPerChunk + 1)
    }
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
    },
    updateMeasurements () {
      this.height = this.$refs.container.clientHeight
      this.itemHeight = 21
      this.itemsPerChunk = Math.ceil(this.height / this.itemHeight)
    },
    updateVirtualization (event) {
      if (!this.virtualize) {
        return
      }
      const y = this.$refs.container.scrollTop
      this.firstVisibleIndex = Math.floor(y / this.itemHeight)
      // const offset = y % this.itemHeight
      // const totalHeight = this.flatNodes.length * this.itemHeight
      const topHidden = Math.max(this.firstVisibleIndex - 1, 0)
      this.paddingTop = topHidden * this.itemHeight
      const bottomHidden = Math.max(this.flatNodes.length - this.firstVisibleIndex - this.itemsPerChunk - 1, 0)
      this.paddingBottom = bottomHidden * this.itemHeight
      console.log(`topHidden: ${topHidden}, bottomHidden: ${bottomHidden}`)
      console.log(this.firstVisibleIndex)
    }
  },
  watch: {
    nodes (value) { this.roots = value }
  },
  created () {
    this.roots = this.nodes
  },
  mounted () {
    if (this.virtualize) {
      this.updateMeasurements()
      this.$refs.container.addEventListener('scroll', this.updateVirtualization)
    }
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
