---
title: 去掉页面的title标题，同时不显示默认链接
tags:
  - 知识点备忘
originContent: ''
categories:
  - 移动开发
  - Web开发
toc: false
date: 2019-08-07 09:43:43
---

有时候我们需要页面的title不显示，尤其在移动端APP内嵌一个网页的时候，有个标题或者标题为空默认显示页面链接都不太友好，这时候有个简单方式可以处理：
```javascript
document.title = '\u200E'
```
加了以后就没了，亲测可行。