---
title: electron开发实践-快速了解electron是什么
tags:
  - 知识点备忘
originContent: >-
  ## 大纲

  - 目的

  - electron能做什么

  - 为什么使用electron

  - 如何更好的使用electron


  ## 目的

  当有人问你能不能开发桌面应用的时候你的回答应该是：“可以，目前我们已经可以使用web技术通过electron框架，创建交互和体验都非常不错的桌面应用了”


  <!-- more -->


  ## electron能做什么

  定义：electron能够使用javascript，html和css构建跨平台的桌面应用。


  从定义上我们知道用js、css、html就能创建桌面应用，同时我们也可以使用vue+webpack等框架来提高开发效率，开发一个桌面应用像开发web页面一样。


  它能够做浏览器做不到的部分，比如更方便的文件操作，获取系统的各种信息和状态等。

  ![image.png](http://blogimage.houjiyi.com/FnHGxVyS5oJfNLKvFYs9ShKLWZqw)


  开源、跨平台，已经有非常多的应用使用electron来开发，它的存在使你不用再为不同平台独立开发而烦恼，它能够一套代码运行在windows、macos、linux三个平台上


  ## 为什么使用electron

  一个工具或者框架的产生一定是解决了某个技术上的痛点，electron也不例外，不同平台的桌面应用使用不同语言进行开发，维护发版等非业务细节工作量繁重。electron能够解决这个问题，或者说他给了一个相对easy的解决方案，一套代码运行在三端，打包发布、自动更新都提供了相应的方式，开发者使用这个框架能够极大提高开发体验，尤其是web开发者。


  原理：用浏览器将web应用打包成一个桌面应用。


  优势：

  1. Web技术

  2. 开源

  3. 跨平台

  4. 化繁为简：自动更新、原生菜单和通知、崩溃报告、调试和性能分析、windows安装程序


  ## 如何更好的使用electron


  既然我们是使用开发web应用的方式来开发electron，这里换句话说就是如何更好的使用web技术来开发electron，我们将最新的前端开发方式与electron结合，也就能够大大提高我们的开发效率，同时技术栈只需要简单扩展就达到了能够开发桌面应用的目的。


  [electron-vue](electron-vue)
  框架目前已经有人完成，其思路就是将vue单页应用的开发方式和electron相结合，使得开发人员在使用electron的时候经常感觉自己是在开发一个web应用。


  只有当与系统进行交互或者打包发版的时候才会用到electron提到的功能。
categories:
  - Web开发
toc: false
date: 2019-07-02 14:08:06
---

## 大纲
- 目的
- electron能做什么
- 为什么使用electron
- 如何更好的使用electron

## 目的
当有人问你能不能开发桌面应用的时候你的回答应该是：“可以，目前我们已经可以使用web技术通过electron框架，创建交互和体验都非常不错的桌面应用了”

<!-- more -->

## electron能做什么
定义：electron能够使用javascript，html和css构建跨平台的桌面应用。

从定义上我们知道用js、css、html就能创建桌面应用，同时我们也可以使用vue+webpack等框架来提高开发效率，开发一个桌面应用像开发web页面一样。

它能够做浏览器做不到的部分，比如更方便的文件操作，获取系统的各种信息和状态等。
![image.png](http://blogimage.houjiyi.com/FnHGxVyS5oJfNLKvFYs9ShKLWZqw)

开源、跨平台，已经有非常多的应用使用electron来开发，它的存在使你不用再为不同平台独立开发而烦恼，它能够一套代码运行在windows、macos、linux三个平台上

## 为什么使用electron
一个工具或者框架的产生一定是解决了某个技术上的痛点，electron也不例外，不同平台的桌面应用使用不同语言进行开发，维护发版等非业务细节工作量繁重。electron能够解决这个问题，或者说他给了一个相对easy的解决方案，一套代码运行在三端，打包发布、自动更新都提供了相应的方式，开发者使用这个框架能够极大提高开发体验，尤其是web开发者。

原理：用浏览器将web应用打包成一个桌面应用。

优势：
1. Web技术
2. 开源
3. 跨平台
4. 化繁为简：自动更新、原生菜单和通知、崩溃报告、调试和性能分析、windows安装程序

## 如何更好的使用electron

既然我们是使用开发web应用的方式来开发electron，这里换句话说就是如何更好的使用web技术来开发electron，我们将最新的前端开发方式与electron结合，也就能够大大提高我们的开发效率，同时技术栈只需要简单扩展就达到了能够开发桌面应用的目的。

[electron-vue](electron-vue) 框架目前已经有人完成，其思路就是将vue单页应用的开发方式和electron相结合，使得开发人员在使用electron的时候经常感觉自己是在开发一个web应用。

只有当与系统进行交互或者打包发版的时候才会用到electron提到的功能。