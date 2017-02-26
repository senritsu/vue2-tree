<template lang="html">
  <div class="tree-node">
    <p>
      <span class="icon is-small" @click="expand">
        <transition name="spin" mode="out-in">
          <i class="fa" :class="nodeIcon" :key="nodeIcon"></i>
        </transition>
      </span>
      {{node.text}}
      <transition name="number">
        <small v-if="collapsed">{{ node.children.length }}</small>
      </transition>
    </p>
    <transition name="children">
      <transition-group tag="div" name="child" class="children" v-if="hasChildren && node.expanded" appear>
        <tree-node v-for="child in node.children" :node="child" :key="child.id"></tree-node>
      </transition-group>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'tree-node',
  props: ['node'],
  computed: {
    collapsed () {
      return this.hasChildren && !this.node.expanded
    },
    hasChildren () {
      return this.node.children && this.node.children.length
    },
    nodeIcon () {
      if (this.hasChildren) {
        return this.node.expanded ? 'fa-minus-square' : 'fa-plus-square'
      } else {
        return 'fa-square'
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
  display: inline-block;
}
.children {
  padding-left: 1rem;
  transition: all 0.4s;
}

.child-move {
  transition: transform 0.2s;
}

.child-enter-active {
  transition: all 0.4s;
}

.child-leave-active {
  position: absolute;
  transition: all 0.2s;
}

.child-enter, .child-leave-to {
  transform: translateX(-1rem);
  opacity: 0;
}

.children-leave-to {
  transform: translateX(-1rem);
  opacity: 0;
  /*position: absolute;*/
}

.number-enter-active, .number-leave-active {
  transition: all 0.2s;
}

.number-enter, .number-leave-to {
  opacity: 0;
  transform: translateX(-0.5rem);
}

.fa-minus-square.spin-enter-active, .fa-plus-square.spin-enter-active {
  transition: transform 0.1s ease-out;
}
.fa-minus-square.spin-leave-active, .fa-plus-square.spin-leave-active {
  transition: transform 0.1s ease-in;
}
.fa-minus-square.spin-enter, .fa-minus-square.spin-leave-to {
  transform: rotate(-45deg);
}
.fa-plus-square.spin-enter, .fa-plus-square.spin-leave-to {
  transform: rotate(45deg);
}
</style>
