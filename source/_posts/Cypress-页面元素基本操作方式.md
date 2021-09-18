---
title: Cypress 页面元素基本操作方式
tags:
  - Web开发
originContent: ''
categories:
  - 自动化测试
toc: false
date: 2021-09-17 12:27:54
---

获取DOM元素基本方式
1. .find(selector) 搜索定位元素
2. .get(selector) 搜索定位元素
3. .contains(selector) 搜索定位元素
4. .children() 方法用来获取DON元素的子元素
5. .parents() 用来获取DOM元素的所有父元素
6. .parent() 用来获取DOM元素第一层元素
7. .siblings() 用来获取DOM元素的所有同级元素
8. .first() 用来获取指定DOM对象的第一个元素
9. .last() 用来获取指定DOM对象的最后一个元素
10. .next() 用来匹配DOM对象紧跟着的下一个同级元素
11. .nextAll() 用来匹配给定的DOM对象的所有同级元素
12. .nextUntil() 用来匹配给定DOM对象之后的所有同级元素直到遇到Until里定义的元素为止
13. .prev() 用来匹配给定DOM对象紧跟着的上一个同级元素
14. .prevAll() 用来匹配给定的DOM对象之前的所有同级元素
15. .prevUntil() 用来匹配给定DOM对象之后的所有同级元素直到遇到Until里定义的元素为止
16. .each() 用来遍历数组及其类似结果
17. .eq() 用来在元素或者数组中的特定索引处获取DOM元素。类似于Jquery中nth:child()


以上就是这篇文章的全部内容了，希望本文的内容对大家的学习或者工作具有一定的参考学习价值，如果有疑问大家可以留言交流，谢谢大家的支持。