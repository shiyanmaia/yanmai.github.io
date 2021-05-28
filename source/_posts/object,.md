---
title: Object.assign()
date: 2021-05-28 20:00:41
tags:
---

`**Object.assign()**` 方法用于将所有可枚举属性的值从一个或多个源对象分配到目标对象。它将返回目标对象。

<!-- more -->

```js
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target);
// expected output: Object { a: 1, b: 4, c: 5 }

console.log(returnedTarget);
// expected output: Object { a: 1, b: 4, c: 5 }
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

## 语法

```
Object.assign(target, ...sources)
```

### 参数

- `target`

  目标对象。

- `sources`

  源对象。

### 返回值

目标对象。

## [描述](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign#描述)

如果目标对象中的属性具有相同的键，则属性将被源对象中的属性覆盖。后面的源对象的属性将类似地覆盖前面的源对象的属性。

`Object.assign` 方法只会拷贝源对象自身的并且可枚举的属性到目标对象。该方法使用源对象的`[[Get]]`和目标对象的`[[Set]]`，所以它会调用相关 getter 和 setter。因此，它分配属性，而不仅仅是复制或定义新的属性。如果合并源包含getter，这可能使其不适合将新属性合并到原型中。为了将属性定义（包括其可枚举性）复制到原型，应使用[`Object.getOwnPropertyDescriptor()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor)和[`Object.defineProperty()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) 。

[`String`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)类型和 [`Symbol`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol) 类型的属性都会被拷贝。

在出现错误的情况下，例如，如果属性不可写，会引发[`TypeError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypeError)，如果在引发错误之前添加了任何属性，则可以更改`target`对象。

注意，`Object.assign` 不会在那些`source`对象值为 [`null`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/null) 或 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined) 的时候抛出错误。