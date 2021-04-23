---
title: Echarts的X轴数据过多时
date: 2021-04-23 16:31:56
tags:
---

 在使用echarts 图表的时候发现要展示的数据过多，数据都挤压在一块

<!-- more -->

```js
yAxis: {
    type: 'category',
    data: ['张小勇1','李思思2','张明明3'],
    axisLabel:{
      interval:0,
       rotate:45
    }
}
```

```js
dataZoom: [{
    type: 'inside',
    start: 50,
    end: 100
}, {
    start: 0,
    end: 10
}],
```

这样设置即可