---
title: 移动设备上Touchmove获取当前触摸的Dom节点
tags:
  - 知识点备忘
originContent: >-
  我们通过touchmove事件返回的event对象，可以获取当前手指触摸元素的clientX和clientY的值，将值传入document.elementFromPoint就可获取当前触摸的值，这里需要注意的是此方法返回的是最顶层的Dom，所以务必将你需要的Dom的z-index设置为最高，分享些许我在此项目的核心代码：


  ```javascript

  /**

  * 获取存储当前Dom位置的对象

  **/

  var myLocation = e.originalEvent.changedTouches[0];


  /**

  * 取clientX及clientY并传入elementFromPoint获取当前手指触摸的Dom

  **/

  var realTarget = document.elementFromPoint(myLocation.clientX,
  myLocation.clientY);

  ```
categories:
  - 移动开发
  - H5开发
toc: false
date: 2019-07-18 09:58:33
---

我们通过touchmove事件返回的event对象，可以获取当前手指触摸元素的clientX和clientY的值，将值传入document.elementFromPoint就可获取当前触摸的值，这里需要注意的是此方法返回的是最顶层的Dom，所以务必将你需要的Dom的z-index设置为最高，分享些许我在此项目的核心代码：

```javascript
/**
* 获取存储当前Dom位置的对象
**/
var myLocation = e.originalEvent.changedTouches[0];

/**
* 取clientX及clientY并传入elementFromPoint获取当前手指触摸的Dom
**/
var realTarget = document.elementFromPoint(myLocation.clientX, myLocation.clientY);
```