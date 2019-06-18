---
title: adonis websocket client报错
tags:
  - javascript
originContent: |-
  在使用adonis websocket client连接的时候浏览器报错：
  ::: hljs-center

  ![image.png](http://blogimage.houjiyi.com/Fo2YsZApigoSxX_TdmCoID8kxdxc)

  :::

  查资料找到了解决办法：
  1. 首先安装@babel/polyfill `npm install --save @babel/polyfill`
  2. 然后在main.js中引入即可
  ::: hljs-center

  ![image.png](http://blogimage.houjiyi.com/Fhoa4uqXbPm8182bvV5FzJzM-Cs7)居中

  :::

  再次运行OK了：
  ::: hljs-center

  ![image.png](http://blogimage.houjiyi.com/Fj9mKFQcK-v_e_LCjCgqpThOG7ri)

  :::
categories:
  - Web开发
toc: false
date: 2019-06-13 16:15:39
---

在使用adonis websocket client连接的时候浏览器报错：
::: hljs-center
![image.png](http://blogimage.houjiyi.com/Fo2YsZApigoSxX_TdmCoID8kxdxc)
:::

查资料找到了解决办法：
1. 首先安装@babel/polyfill `npm install --save @babel/polyfill`
2. 然后在main.js中引入即可
::: hljs-center
![image.png](http://blogimage.houjiyi.com/Fhoa4uqXbPm8182bvV5FzJzM-Cs7)居中
:::

再次运行OK了：
::: hljs-center
![image.png](http://blogimage.houjiyi.com/Fj9mKFQcK-v_e_LCjCgqpThOG7ri)
:::