---
title: vue响应式自适应高度
date: 2021-04-07 19:55:37
tags:
---
<!-- more -->
	在
	componentHeight: 0,
	componentWidth: 0,
中
    mounted: function() {
    // TODO: how to recalculate the component height?
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
  },

   methods: 中
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
      // 因为可能存在组件初始化后在短时间被销毁，或者在一定时机触发这个计算时，组件正在销毁中，不能让这个引用导致内存溢出
      // 还有一种可能是连续调用计算高度，未清除上一个还在计算的timer
    },



    引入一个插件js
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

在页面中
calcHeight:true,设置
便可以监控到componentHeight