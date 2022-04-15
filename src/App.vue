<template>
  <div id="app" style="width: 300px; height: 200px; border: 1px solid royalblue;">
    <ContextMenu scope="app" style="width: 100px; height: 20px; background: yellow;">
      广告位招租
    </ContextMenu>
  </div>
</template>

<script>
import ContextMenu from '@/components/ContextMenu.vue'

export default {
  components: {
    ContextMenu,
  },
  mounted() {
    document.oncontextmenu = function () {
      return false
    }
  }
}
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }
</style>
