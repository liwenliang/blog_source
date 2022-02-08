---
title: electron ipcRenderer引用方式引发的报错
tags:
  - 知识点备忘
  - electron
originContent: ''
categories:
  - Web开发
toc: false
date: 2022-02-08 15:03:19
---


通过`import { ipcRenderer } from 'electron';`这种方式引用会报错如下`TypeError: path.join is not a function`：

![image.png](https://blogimage.houjiyi.com/Fhh9uMGzklsPsWoFMwPXDyi-9S3a)

修改引用方式为：`const { ipcRenderer } = window.require('electron');`即可；
