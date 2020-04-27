<template>
  <div id="app" :class="mode">
    <side-bar @order="setOrder" @mode="setMode"/>
    <content-cards :order="order" />
  </div>
</template>

<script>
import SideBar from '@/components/SideBar.vue'
import ContentCards from '@/components/Content.vue'

export default {
  name: 'App',
  components: {
    SideBar,
    ContentCards
  },
  data () {
    return {
      order: 'default',
      render: 0,
      mode: 'light'
    }
  },
  methods: {
    setOrder (order) {
      this.render++
      this.order = `${order}-${this.render}`
    },
    setMode (mode) {
      this.mode = mode
    }
  }
}
</script>

<style lang="scss">
#app {
  display: flex;
  color: #3b3b3b;
  background-color: #fff;
  &.dark {
    color: #fff;
    background-color: #000;
  }
}
</style>
