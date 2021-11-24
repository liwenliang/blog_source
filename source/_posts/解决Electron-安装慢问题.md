---
title: 解决Electron 安装慢问题
tags:
  - 开发调试
categories:
  - Web开发
toc: false
date: 2021-10-26 19:00:50
---

安装Electron时会判断缓存中是否有要下载的zip包，如果没有的话就去下载这个zip包。但是往往下载这个zip包时非常慢，总是下载不成功。下面给出我的解决办法：

首先要知道缓存位置在哪，根据 electron-download的描述 缓存的位置取决于操作系统 默认值为：

> Linux：$XDG_CACHE_HOME或~/.cache/electron/
苹果系统：~/Library/Caches/electron/
Windows：$LOCALAPPDATA/electron/Cache或~/AppData/Local/electron/Cache/

然后，[下载](https://npm.taobao.org/mirrors/electron)需要的zip版本的包,放到上面提到的缓存地址中。

最后，在命令窗口中执行npm install electron -g,执行成功

![image.png](https://blogimage.houjiyi.com/Fh0vqiZD8HtNUziLZRNzjMc_EhYQ)

