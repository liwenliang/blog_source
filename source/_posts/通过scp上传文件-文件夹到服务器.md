---
title: 通过scp上传文件/文件夹到服务器
tags:
  - linux
  - 知识点备忘
originContent: ''
categories:
  - Web开发
toc: false
date: 2021-09-29 13:56:03
---

```bash
// 上传
scp 文件 username@ip:/目录
scp -r 文件夹 username@ip:/目录

// 下载
scp username@ip:/目录  本地目录

通过ssh username@ip 可连接服务器查看
```