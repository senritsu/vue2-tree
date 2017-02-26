<template lang="html">
  <div class="tree-node" :style="{marginLeft: `${level}rem`}">
    <p>
      <span class="icon is-small" @click="expand">
        <transition name="spin" mode="out-in">
          <i class="fa" :class="nodeIcon" :key="nodeIcon"></i>
        </transition>
      </span>
      {{node.text}} ({{index}})
      <transition name="number">
        <small v-if="collapsed">{{ node.children.length }}</small>
      </transition>
    </p>
  </div>
</template>

<script>
export default {
  props: ['node', 'level', 'index'],
  computed: {
    collapsed () {
      return this.hasChildren && !this.node.expanded
    },
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
      this.$emit('expanded')
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
  display: inline-block;
}

.number-enter-active, .number-leave-active {
  transition: all 0.2s;
}

.number-enter, .number-leave-to {
  opacity: 0;
  transform: translateX(-0.5rem);
}

.fa-minus-square-o.spin-enter-active, .fa-plus-square-o.spin-enter-active {
  transition: transform 0.1s ease-out;
}
.fa-minus-square-o.spin-leave-active, .fa-plus-square-o.spin-leave-active {
  transition: transform 0.1s ease-in;
}
.fa-minus-square-o.spin-enter, .fa-minus-square-o.spin-leave-to {
  transform: rotate(-45deg);
}
.fa-plus-square-o.spin-enter, .fa-plus-square-o.spin-leave-to {
  transform: rotate(45deg);
}
</style>
