---
title: 10分钟实现微信 "炸屎"大作战
date: 2021-06-04 13:46:42
math: true
tags:
---

## 步骤

1.**丢炸弹**

轨迹路径（类似 y = x2x^2x2 ），然后通过`tween.js`来做补间动画。

<!-- more -->

2.**炸弹爆炸**

利用`lottie` 来实现动画。

3.**粑粑被炸开**

利用 `css` 动画实现

4.**所有人震动**

利用 `css` 动画实现

------

## 具体实现

### 1.丢炸弹

现在假设我们的炸弹是一个 10px * 10px 的小方块，设置起始点为（300，300）终点为 （0，100） H=100，此时我们得到的二次函数为：
$$
y=(1/120)x^2-(11/6)x+100
$$

而渲染每一帧动画，我们则用了著名的补间动画库[Tween.js](http://tweenjs.github.io/tween.js) 补间(动画)是一个概念，允许你以平滑的方式更改对象的属性。你只需告诉它哪些属性要更改，当补间结束运行时它们应该具有哪些最终值，以及这需要多长时间，补间引擎将负责计算从起始点到结束点的值。

```js
var coords = { x: 300 };  // 起始点 为 x = 300
var tween = new TWEEN.Tween(coords)
 .to({ x: 0  }, 1000) // 终点为 x = 0, 并且这个动作将在1秒内完成
  .easing(TWEEN.Easing.Linear.None) // 匀速

```

完整demo   [效果链接](/demo)

```html
<!DOCTYPE html>
<html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>cdn-animation</title>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/tween.js/16.7.0/Tween.js"></script>
    </head>

    <body>
        <script>
            var box = document.createElement('div');
            box.style.setProperty('width', '20px');
            box.style.setProperty('height', '20px');
            box.style.setProperty('background-position', 'center');
            box.style.setProperty('background-size', '100%');
            box.style.setProperty('background-image', 'url(./bomb.png)');
            box.style.setProperty('transform', 'translate(' + 300 + 'px, ' + 300 + 'px)');
            document.body.appendChild(box);

            function animate(time) {
                requestAnimationFrame(animate);
                TWEEN.update(time);
            }
            requestAnimationFrame(animate);

            var H = 100;

            var coords = { x: 300, y: 0 };
            var tween = new TWEEN.Tween(coords)
            .to({ x: 0, y: 360 }, 1000)
            .easing(TWEEN.Easing.Linear.None)
            .onUpdate(function () {
                var c = H;
                var a = 5 / (6 * H);
                var b = -11 / 6;
                var x = coords.x;
                var y = a * x * x + b * x + c;
                console.log(coords.y);
                box.style.setProperty('transform', `translate(${x}px, ${y}px) rotate(${coords.y}deg)`);
                // box.style.setProperty('transform', `rotate(${coords.y}deg)`);
            })
            .start();
        </script>
    </body>

</html>
```
