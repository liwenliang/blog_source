---
title: npm使用淘宝镜像
tags:
  - nodejs
categories:
  - NodeJS
toc: false
date: 2017-09-22 18:21:51
---

> 淘宝的npm镜像地址为：[https://registry.npm.taobao.org](https://registry.npm.taobao.org) 

<!-- more -->

# 临时使用
```
npm --registry https://registry.npm.taobao.org install xxx
```

# 永久使用
```
npm config set registry https://registry.npm.taobao.org
```

配置后使用npm config get registry来看是否已经发生变化

还有一种方式是通过使用cnpm，但是这种方式会有一个问题，通过cnpm安装的组件很多会在目录前边带有版本号，个人认为这个效果很差，由于它是一个软连接的方式进行的引用，在windows系统甚至会报错，所以在无法翻墙的情况下，使用永久的用法还是不错的。
