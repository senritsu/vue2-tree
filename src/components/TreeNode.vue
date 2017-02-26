<template lang="html">
  <div class="tree-node">
    <p>
      <span class="icon is-small" @click="expand">
        <i class="fa" :class="nodeIcon"></i>
      </span>
      {{node.text}}
      <small v-if="hasChildren && !node.expanded">{{ node.children.length }}</small>
    </p>
    <div class="children" v-if="hasChildren && node.expanded">
      <tree-node v-for="child in node.children" :node="child"></tree-node>
    </div>
  </div>
</template>

<script>
export default {
  name: 'tree-node',
  props: ['node'],
  computed: {
    hasChildren () {
      return this.node.children && this.node.children.length
    },
    nodeIcon () {
      if (this.hasChildren) {
        return this.node.expanded ? 'fa-minus-square-o' : 'fa-plus-square-o'
      } else {
        return 'fa-square-o'
      }
    }
  },
  methods: {
    expand () {
      if (!this.hasChildren) {
        return
      }

      this.$set(this.node, 'expanded', !this.node.expanded)
    }
  }
}
</script>

<style lang="css" scoped>
.icon {
  margin-top: 3px;
}
.tree-node {
  text-align: left;
}
small {
  opacity: 0.3;
}
.children {
  padding-left: 1rem;
}
</style>
