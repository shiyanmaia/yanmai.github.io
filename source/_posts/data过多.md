---
title: el-select大数据量渲染慢导致页面卡顿的问题
date: 2021-06-11 17:03:36
categories: 前端性能优化

---

一次性渲染大量数据问题

<!--more-->

```js
<template>
    <div class="content">
         <el-select v-model="choose" size="small" v-el-select-loadmore:rangeNumber="loadMore(rangeNumber)">
             <el-option
             v-for="(item, index) in list.slice(0, rangeNumber)" 
             :key="index"
             :label="item.label"
             :value="item.value"></el-option>
         </el-select>
    </div>
</template>
 
<script>
import Vue from "vue";
Vue.directive(
    'el-select-loadmore', {
        bind(el, binding) {
            let self = this
            // 获取element-ui定义好的scroll盒子
            const SELECTWRAP_DOM = el.querySelector('.el-select-dropdown .el-select-dropdown__wrap');
            SELECTWRAP_DOM.addEventListener('scroll', function () {
                /**
                * scrollHeight 获取元素内容高度(只读)
                * scrollTop 获取或者设置元素的偏移值,常用于, 计算滚动条的位置, 当一个元素的容器没有产生垂直方向的滚动条, 那它的scrollTop的值默认为0.
                * clientHeight 读取元素的可见高度(只读)
                * 如果元素滚动到底, 下面等式返回true, 没有则返回false:
                * ele.scrollHeight - ele.scrollTop === ele.clientHeight;
                */
                const condition = this.scrollHeight - this.scrollTop <= this.clientHeight;
                if (condition) binding.value()
            });
        }
    }
)
export default {
    data() {
        return {
           list: [],
           choose: "",
           rangeNumber: 10,
        }
    },
    created(){
       this.getList()
    },
    methods: {
        getList(){
            for(let i = 0; i < 100000; i++){
                this.list.push({
                   label: "menu",
                   value: "menu"
                })
            }//测试数据10万条数据, 这里数据多少条无所谓,list.slice(0, rangeNumber)方法只会默认加载初始的10条数据
        },
        loadMore(n){
            //n是默认初始展示的条数会在渲染的时候就可以获取,具体可以打log查看
            //if(n < 8) this.rangeNumber = 10 //elementui下拉超过7条才会出滚动条,如果初始不出滚动条无法触发loadMore方法
            return () => this.rangeNumber += 5 //每次滚动到底部可以新增条数  可自定义
        },
    }
}
</script>
```

