---
title: 本地图片不显示问题
date: 2021-04-07 22:54:11
tags:
categories: hexo的问题
---
今天想测试使用md加入图片，发现npm build以后的路径会出错，查百度是说由于hexo的版本升级后导致路径不对，可以使用修改后的hexo-asset-image插件来解决这个问题.。<!-- more -->将修改好的仓库代码fork到了自己的git仓库，使用

`npm install https://github.com/shiyanmaia/hexo-asset-image.git --save`

来进行安装，git推送以后如果使用`![](相对路径)`来显示图片会使用一个奇怪的路径，且使用的是github的绝对路径，经过排查发现是_config.yml的配置错误，其中URL的配置没有配置对

{% asset_img 1.jpg %}

如上图修改正确以后，便可以使用`![](相对路径)`来引入图片了，官方引入图片方法是

```markdown
{% asset_img test.jpg [width] [height] This is an test image %}
```
不要使用中文路径，虽然经过验证，中文.md也可以正常使用，但是插入图片的话可能会有错误。
