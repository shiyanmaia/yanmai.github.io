---
title: 通用的前端复制方法
date: 2021-05-14 15:09:41
tags:
---

前端通用的copy方法:

```js
copyUrl() {
    const aux = document.createElement('input')
    aux.setAttribute('value', this.loginHerf)// this.loginHerf是需要copy的东西
    document.body.appendChild(aux)
    aux.select()
    document.execCommand('copy')
    document.body.removeChild(aux)
    this.$message({
        type: 'success',
        message: '复制成功!'
    })
},
```

或者

使用插件clipboard.js

使用插件v-clipboard

```js
//安装
npm install --save v-clipboard

//在main.js中引入
import Clipboard from 'v-clipboard'
Vue.use(Clipboard)

//使用
<button v-clipboard="value">
  Copy to clipboard
</button>
或者
<button v-clipboard="() => value">
  Copy to clipboard
</button>
或者
this.$clipboard(value)

//回调事件
<template>
    <button v-clipboard="foo"
            v-clipboard:success="clipboardSuccessHandler" // Success event handler 
            v-clipboard:error="clipboardErrorHandler">    // Error event handler
      Copy to clipboard
    </button>
</template>
<script>
export default{
    methods: {
    clipboardSuccessHandler ({ value, event }) {
      console.log('success', value)
    },

    clipboardErrorHandler ({ value, event }) {
      console.log('error', value)
    }
  }
}
</script>
```

