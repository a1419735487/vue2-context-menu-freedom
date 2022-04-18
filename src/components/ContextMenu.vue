<template>
  <div id="sf-context-menu" :style="{left: left, top: top, opacity: opacity, position: 'fixed'}" v-show="show">
    <slot></slot>
  </div>
</template>

<script>

export default {
  props: {
    scope: {
      type: String,
      default: ''   // 标签id，传空为全局
    }
  },
  data() {
    return {
      dom: undefined,
      left: 0,
      top: 0,
      opacity: 0,
      show: false
    }
  },
  mounted() {
    this.initContextMenu()
  },
  methods: {
    initContextMenu() {
      // 获取作用域dom
      try {
        if (this.scope === '') this.dom = document.body
        else this.dom = document.getElementById(`${this.scope}`)
      } catch {
        console.error('[error]找不到该dom元素，请检查')
      }
      this.dom.addEventListener('contextmenu', this.contextMenu)
    },
    contextMenu() {
      // 先把菜单藏起来
      this.show = true
      this.opacity = 0
      this.$nextTick(() => {
        // 计算右键菜单和容器边界位置
        const evt = window.event || arguments[0]
        const contextMenu = document.getElementById('sf-context-menu')
        if (contextMenu.clientWidth > this.dom.getBoundingClientRect().width) {
          console.error('[error]菜单宽度大于容器宽度，请检查')
          return
        } else if (contextMenu.clientHeight > this.dom.getBoundingClientRect().height) {
          console.error('[error]菜单高度大于容器高度，请检查')
          return
        } else {
          let rightEdge = evt.clientX + contextMenu.clientWidth
          let bottomEdge = evt.clientY + contextMenu.clientHeight
          let domRight = this.dom.getBoundingClientRect().left + this.dom.clientWidth
          let domBottom = this.dom.getBoundingClientRect().top + this.dom.clientHeight
          // 边界判断与定位赋值
          this.left = rightEdge > domRight ? evt.clientX - (rightEdge - domRight) + 'px' : evt.clientX + 'px'
          this.top = bottomEdge > domBottom ? evt.clientY - (bottomEdge - domBottom) + 'px' : evt.clientY + 'px'
          // 可以展示菜单了
          this.opacity = 1
          // 再点一下关闭菜单框
          window.addEventListener('mouseup', this.closeMenu)
        }
      })
    },
    closeMenu() {
      this.show = false
      this.opacity = 0
    }
  },
  beforeDestroy() {
    this.dom.removeEventListener('contextmenu', this.contextMenu())
    window.removeEventListener('mouseup', this.closeMenu())
  }
}
</script>