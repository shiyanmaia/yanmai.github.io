---
title: 13种Vue修饰符
date: 2021-07-07 16:48:07
---
1. lazy

   `lazy`修饰符作用是，改变输入框的值时value不会改变，当光标离开输入框时，`v-model`绑定的值value才会改变

   <input type="text" v-model.lazy="value">

2. trim

   `trim`修饰符的作用类似于JavaScript中的`trim()`方法，作用是把`v-model`绑定的值的首尾空格给过滤掉。

3. number

   `number`修饰符的作用是将值转成数字，但是先输入字符串和先输入数字，是两种情况

   <input type="text" v-model.number="value">

   先输入数字的话，只取前面数字部分

   先输入字母的话，`number`修饰符无效

4. stop

   `stop`修饰符的作用是阻止冒泡

   ```html
   <div @click="clickEvent(2)" style="width:300px;height:100px;background:red">
     <button @click.stop="clickEvent(1)">点击</button>
   </div>
   ```

5. capture

   事件默认是由里往外`冒泡`，`capture`修饰符的作用是反过来，由外网内`捕获`

6. self

   `self`修饰符作用是，只有点击事件绑定的本身才会触发事件

7. once

   `once`修饰符的作用是，事件只执行一次

8. prevent

   `prevent`修饰符的作用是阻止默认事件（例如a标签的跳转）

9. native

   `native`修饰符是加在自定义组件的事件上，保证事件能执行

10. left，right，middle
    这三个修饰符是鼠标的左中右按键触发的事件

    <button @click.middle="clickEvent(1)"  @click.left="clickEvent(2)"  @click.right="clickEvent(3)">点我</button>

11. passive
    当我们在监听元素滚动事件的时候，会一直触发onscroll事件，在pc端是没啥问题的，但是在移动端，会让我们的网页变卡，因此我们使用这个修饰符的时候，相当于给onscroll事件整了一个.lazy修饰符

12. camel
    不加camel viewBox会被识别成viewbox

    `<svg :viewBox="viewBox"></svg>`

    加了canmel viewBox才会被识别成viewBox

    `<svg :viewBox.camel="viewBox"></svg>`

13. sync
    当父组件传值进子组件，子组件想要改变这个值时，可以这么做

    ```js
    //父组件里
    <children :foo="bar" @update:foo="val => bar = val"></children>
    
    //子组件里
    this.$emit('update:foo', newValue)
    
    //sync修饰符的作用就是，可以简写：
    
    //父组件里
    <children :foo.sync="bar"></children>
    
    //子组件里
    this.$emit('update:foo', newValue)
    ```

14. keyCode
    当我们这么写事件的时候，无论按什么按钮都会触发事件
    `<input type="text" @keyup="shout(4)">`
    那么想要限制成某个按键触发怎么办？这时候keyCode修饰符就派上用场了
    `<input type="text" @keyup.keyCode="shout(4)">`

    ```
    Vue提供的keyCode：
    //普通键
    .enter 
    .tab
    .delete //(捕获“删除”和“退格”键)
    .space
    .esc
    .up
    .down
    .left
    .right
    //系统修饰键
    .ctrl
    .alt
    .meta
    .shift
    
    按 ctrl 才会触发
    <input type="text" @keyup.ctrl="shout(4)">
    
    也可以鼠标事件+按键
    <input type="text" @mousedown.ctrl.="shout(4)">
    
    可以多按键触发 例如 ctrl + 67
    <input type="text" @keyup.ctrl.67="shout(4)">
    ```

    


