---
title: 右箭头CSS
date: 2021-05-07 13:48:58
tags:
---

右箭头的样式，可以用>这个，也可以用css，css更容易更改样式和调整。

```js
.right-arrow {  
    display :inline-block;
    position: relative;
    width: 14px;
    height: 26px;
    margin-right: 20px;
}
.right-arrow::after {
    display: inline-block;
    content: " ";
    height: 13px;
    width: 13px;
    border-width: 3px 3px 0 0;
    border-color: #979797;
    border-style: solid;
    transform: matrix(0.71, 0.71, -0.71, 0.71, 0, 0);
    position: absolute;
    top: 50%;
    left: 700 * @base;
}
```

