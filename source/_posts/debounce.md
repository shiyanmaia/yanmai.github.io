---
title: 防抖节流
date: 2021-04-13 17:03:36
tags:前端性能优化
---

被触发的频次非常高，间隔很近,有可能造成其他的问题，这个时候需要防抖节流

###### 函数防抖

规定函数至少间隔多久执行。

- 函数执行过一次后，在n秒内不能再次执行，否则推迟函数执行

- 下一次函数调用将清除上一次的定时器，并用setTimeout重新计时

  

| 例子                                 | 未使用函数防抖               | 使用函数防抖               |
| ------------------------------------ | ---------------------------- | -------------------------- |
| 当窗口大小发生变化时计算新的窗口大小 | 窗口大小改变就计算大小       | 窗口大小改变完成后计算大小 |
| 验证用户输入                         | 用户每输入一个字符就进行验证 | 用户输入完成后进行验证     |

###### 函数节流

规定函数在某时间段内最多执行一次

- 函数在n秒内最多执行一次
- 下一次函数调用将清除上一次的定时器
- 若函数执行的时间间隔小于等于规定时间间隔则用setTimeout在规定时间后再执行
- 若函数执行的时间间隔大于规定时间间隔则执行函数，并重新计时



| 例子               | 未使用函数节流           | 使用函数节流               |
| ------------------ | ------------------------ | -------------------------- |
| 页面滚动图片懒加载 | 每次都会执行事件处理程序 | 指定事件处理程序的执行频率 |



何时使用函数防抖、何时使用函数节流看需求：

- 当我们只需要处理最后一次触发事件时，用函数防抖。如窗口大小值变化时并不需要计算中间变化的过程，只需要窗口大小改变完成后的值
- 当事件触发过于频繁，我们需要限制事件处理程序的调用频率时，用函数节流。

```js
// 防抖
export function _debounce(fn, delay) {

    var delay = delay || 200;
    var timer;
    return function () {
        var th = this;
        var args = arguments;
        if (timer) {
            clearTimeout(timer);
        }
        timer = setTimeout(function () {
            timer = null;
            fn.apply(th, args);
        }, delay);
    };

}
```

```js
// 节流
export function _throttle(fn, interval) {
    var last;
    var timer;
    var interval = interval || 200;
    return function () {
        var th = this;
        var args = arguments;
        var now = +new Date();
        if (last && now - last < interval) {
            clearTimeout(timer);
            timer = setTimeout(function () {
                last = now;
                fn.apply(th, args);
            }, interval);
        } else {
            last = now;
            fn.apply(th, args);
        }
    }
}
```

在需要使用的组件引用

```js
import { _debounce } from "...";
```

在 `methods` 中使用

```js
  methods: {
    changefield: _debounce(function(_type, index, item) {
        // do something ...
    }, 200)
  }
```