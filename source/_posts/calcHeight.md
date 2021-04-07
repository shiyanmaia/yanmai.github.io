---
title: vue响应式自适应高度
date: 2021-04-07 19:55:37
tags:
---
解决vue中组件不能实时更新高度问题
<!-- more -->
在vue.mixin中添加
  ``` js
	componentHeight: 0,
	componentWidth: 0,
  ```
  mounted中添加
  ``` js
    mounted: function() {
    if (this.$options.calcHeight) {
      this.calcHeight()
      const resizeHandler = util.debounce(() => {
        this.calcHeight()
      }, 200)
      window.addEventListener('resize', resizeHandler)
      this.$on('hook:beforeDestroy', () => {
        window.removeEventListener('resize', resizeHandler)
      })
    }
  }
  ```
  methods中添加
```js
    calcHeight() {
      if (!this.$options.calcHeight) {
        return
      }
      clearInterval(this._inner_timer)
      this._inner_timer = setInterval(() => {
        if (this.$el && this.$el.getClientRects && this.$el.getClientRects() && this.$el.getClientRects()[0]) {
          const h = this.$el.getClientRects()[0].height
          if (!isNaN(h)) {
            clearInterval(this._inner_timer)
            this.componentHeight = h
            this.componentWidth = this.$el.getClientRects()[0].width
            if (this.$options.calcHeightSuccess) {
              this[this.$options.calcHeightSuccess]()
            }
          }
        }
      }, 10)
    },
```
引入一个插件js
```js
export default {
  debounce (func, wait) {
    let timeout, args, context, timestamp, result
    const later = () => {
      const last = Date.now() - timestamp
      if (last < wait && last > 0) {
        timeout = setTimeout(later, wait - last)
      } else {
        timeout = null
        result = func.apply(context, args)
        context = args = null
      }
    }
    return function (...argArr) {
      context = this
      args = argArr
      timestamp = Date.now()
      if (!timeout) { // 延时不存在，即第一次调用
        timeout = setTimeout(later, wait)
      }
      return result
    }
  } 
} 
```
接下来在需要使用计算的页面中设置`calcHeight:true`,然后便可以监控到`componentHeight`的变化