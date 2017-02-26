<template lang="html">
  <div class="tree-node" :style="{marginLeft: `${level}rem`}">
    <p>
      <span class="icon is-small" @click="expand">
        <transition name="spin" mode="out-in">
          <i class="fa" :class="nodeIcon" :key="nodeIcon"></i>
        </transition>
      </span>
      {{node.text}}
      <small v-if="hasChildren && !node.expanded">{{ node.children.length }}</small>
    </p>
  </div>
</template>

<script>
export default {
  props: ['node', 'level'],
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
.fa-minus-square-o.spin-enter-active, .fa-plus-square-o.spin-enter-active {
  transition: transform 0.15s ease-out;
}
.fa-minus-square-o.spin-leave-active, .fa-plus-square-o.spin-leave-active {
  transition: transform 0.15s ease-in;
}
.fa-minus-square-o.spin-enter, .fa-minus-square-o.spin-leave-to {
  transform: rotate(-45deg);
}
.fa-plus-square-o.spin-enter, .fa-plus-square-o.spin-leave-to {
  transform: rotate(45deg);
}
</style>
