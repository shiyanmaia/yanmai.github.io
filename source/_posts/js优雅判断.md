---
title: js如何少写if else
date: 2021-06-11 10:46:42
tags:

---

```js
/**
 * 按钮点击事件
 * @param {number} status 活动状态：1 开团进行中 2 开团失败 3 商品售罄 4 开团成功 5 系统取消
 */
const onButtonClick = (status)=>{
    if(status == 1){
        sendLog('processing')
        jumpTo('IndexPage')
    }else if(status == 2){
        sendLog('fail')
        jumpTo('FailPage')
    }
```

改为switch的模式

```js
/**
 * 按钮点击事件
 * @param {number} status 活动状态：1 开团进行中 2 开团失败 3 商品售罄 4 开团成功 5 系统取消
 */
const onButtonClick = (status)=>{
    switch (status){
        case 1:
            sendLog('processing')
            jumpTo('IndexPage')
            break
        case 2:
        case 3:
            sendLog('fail')
            jumpTo('FailPage')
            break
        default:
            sendLog('other')
            jumpTo('Index')
            break
    }
```

case 2和case 3逻辑一样的时候，可以省去执行语句和break，则case 2的情况自动执行case 3的逻辑。

再次简化

```js
const actions = {
    '1': ['processing','IndexPage'],
    '2': ['fail','FailPage'],
    '3': ['fail','FailPage'],
    '4': ['success','SuccessPage'],
    '5': ['cancel','CancelPage'],
    'default': ['other','Index'],
}
/**
 * 按钮点击事件
 * @param {number} status 活动状态：1开团进行中 2开团失败 3 商品售罄 4 开团成功 5 系统取消
 */
const onButtonClick = (status)=>{
    let action = actions[status] || actions['default'],
        logName = action[0],
        pageName = action[1]
    sendLog(logName)
    jumpTo(pageName)
}
```

再次优化

```js
const actions = new Map([
    [1, ['processing','IndexPage']],
    [2, ['fail','FailPage']],
    [3, ['fail','FailPage']],
    [4, ['success','SuccessPage']],
    [5, ['cancel','CancelPage']],
    ['default', ['other','Index']]
])
/**
 * 按钮点击事件
 * @param {number} status 活动状态：1 开团进行中 2 开团失败 3 商品售罄 4 开团成功 5 系统取消
 */
const onButtonClick = (status)=>{
    let action = actions.get(status) || actions.get('default')
    sendLog(action[0])
    jumpTo(action[1])
}

```

Map对象和Object对象有什么区别呢？

1. 一个对象通常都有自己的原型，所以一个对象总有一个"prototype"键。
2. 一个对象的键只能是字符串或者Symbols，但一个Map的键可以是任意值。
3. 你可以通过size属性很容易地得到一个Map的键值对个数，而对象的键值对个数只能手动确认。

这个时候如果有二元判断

```js
if（1）{
    if（1）{
        //do sth
    }
}
```

可以这么做

```js
const actions = new Map([
    ['guest_1', ()=>{/*do sth*/}],
    ['guest_2', ()=>{/*do sth*/}],
    ['guest_3', ()=>{/*do sth*/}],
    ['guest_4', ()=>{/*do sth*/}],
    ['guest_5', ()=>{/*do sth*/}],
    ['master_1', ()=>{/*do sth*/}],
    ['master_2', ()=>{/*do sth*/}],
    ['master_3', ()=>{/*do sth*/}],
    ['master_4', ()=>{/*do sth*/}],
    ['master_5', ()=>{/*do sth*/}],
    ['default', ()=>{/*do sth*/}],
])

/**
 * 按钮点击事件
 * @param {string} identity 身份标识：guest客态 master主态
 * @param {number} status 活动状态：1 开团进行中 2 开团失败 3 开团成功 4 商品售罄 5 有库存未开团
 */
const onButtonClick = (identity,status)=>{
    let action = actions.get(`${identity}_${status}`) || actions.get('default')
    action.call(this)
}
```

用Object对象来实现也是类似的，Map可以用任何类型的数据作为key。

```js
const actions = {
  'guest_1':()=>{/*do sth*/},
  'guest_2':()=>{/*do sth*/},
  //....
}

const onButtonClick = (identity,status)=>{
  let action = actions[`${identity}_${status}`] || actions['default']
  action.call(this)
}
```

假如guest情况下，status1-4的处理逻辑都一样怎么办

最差的情况是这样

```js
const actions = new Map([
  [{identity:'guest',status:1},()=>{/* functionA */}],
  [{identity:'guest',status:2},()=>{/* functionA */}],
  [{identity:'guest',status:3},()=>{/* functionA */}],
  [{identity:'guest',status:4},()=>{/* functionA */}],
  [{identity:'guest',status:5},()=>{/* functionB */}],
  //...
])
```

好一点的写法是将处理逻辑函数进行缓存

```js
const actions = ()=>{
  const functionA = ()=>{/*do sth*/}
  const functionB = ()=>{/*do sth*/}
  return new Map([
    [{identity:'guest',status:1},functionA],
    [{identity:'guest',status:2},functionA],
    [{identity:'guest',status:3},functionA],
    [{identity:'guest',status:4},functionA],
    [{identity:'guest',status:5},functionB],
    //...
  ])
}

const onButtonClick = (identity,status)=>{
  let action = [...actions()].filter(([key,value])=>(key.identity == identity && key.status == status))
  action.forEach(([key,value])=>value.call(this))
}
```

优化

```js
const actions = ()=>{
  const functionA = ()=>{/*do sth*/}
  const functionB = ()=>{/*do sth*/}
  return new Map([
    [/^guest_[1-4]$/,functionA],
    [/^guest_5$/,functionB],
    //...
  ])
}

const onButtonClick = (identity,status)=>{
  let action = [...actions()].filter(([key,value])=>(key.test(`${identity}_${status}`)))
  action.forEach(([key,value])=>value.call(this))
}
```

假如需求变成，凡是guest情况都要发送一个日志埋点，不同status情况也需要单独的逻辑处理，那我们可以这样写：

```js
const actions = ()=>{
  const functionA = ()=>{/*do sth*/}
  const functionB = ()=>{/*do sth*/}
  const functionC = ()=>{/*send log*/}
  return new Map([
    [/^guest_[1-4]$/,functionA],
    [/^guest_5$/,functionB],
    [/^guest_.*$/,functionC],
    //...
  ])
}

const onButtonClick = (identity,status)=>{
  let action = [...actions()].filter(([key,value])=>(key.test(`${identity}_${status}`)))
  action.forEach(([key,value])=>value.call(this))
}
```

总结

8种逻辑判断写法，包括：

1. if/else

2. switch

3. 一元判断时：存到Object里

4. 一元判断时：存到Map里

5. 多元判断时：将condition拼接成字符串存到Object里

6. 多元判断时：将condition拼接成字符串存到Map里

7. 多元判断时：将condition存为Object存到Map里

8. 多元判断时：将condition写作正则存到Map里

   

原文：https://juejin.cn/post/6844903705058213896

