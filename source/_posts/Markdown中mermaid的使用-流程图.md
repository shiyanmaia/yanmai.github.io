---
title: Markdown中mermaid的使用(流程图)
date: 2021-04-07 14:45:47
tags:
mermaid: true
---
Markdown 语法并不直接支持画图
但是可以引入mermaid插件

```markdown
```mermaid
graph LR;
　　Portal-->|发布/更新配置|Apollo配置中心;
　　Apollo配置中心-->|实时推送|App;
　　App-->|实时查询|Apollo配置中心;
``` ```

``` mermaid
graph LR;  
　　A-->B;    
　　A-->C;  
　　B-->D;  
　　C-->D;
```
<font size=2>
具体使用参考:

https://blog.csdn.net/fenghuizhidao/article/details/79440583
https://www.cnblogs.com/nanqiang/p/8244309.html

</font>
