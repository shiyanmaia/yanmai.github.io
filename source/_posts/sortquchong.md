---
title: 前端去除数组中重复对象
date: 2021-05-24 14:37:41
tags:
---

两个函数

<!-- more -->

```js
const deteleObject = (obj) => {
    let uniques = []
    let stringify = {}
    for (let i = 0; i < obj.length; i++) {
        let keys = Object.keys(obj[i])
        keys.sort(function (a, b) {
            return (Number(a) - Number(b))
        })
        let str = ''
        for (let j = 0; j < keys.length; j++) {
            str += JSON.stringify(keys[j])
            str += JSON.stringify(obj[i][keys[j]])
        }
        if (!stringify.hasOwnProperty(str)) {
            uniques.push(obj[i])
            stringify[str] = true
        }
    }
    return uniques
}
```

暂未使用的方法：

```js
var hash = {}
arr = arr.reduce(function(item, next) { 
    hash[next.name] ? '' : hash[next.name] = true && item.push(next) 
    return item
}, [])
```

