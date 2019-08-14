---
title: centos7对普通用户授权sudo权限
tags:
  - 知识点备忘
  - jenkins
  - linux
originContent: ''
categories:
  - Web开发
toc: false
date: 2019-08-14 15:18:17
---

centos7 默认普通用户没有sudo权限，因此需要我们手动设置
步骤： 
1、su进入root用户，`vi /etc/sudoers `
2、找到文件中的下列文字： 
`###Allow root to run any commands anywhere `
`root ALL=(ALL) ALL`
在该行下面添加如下即可 
`xxx ALL=(ALL) ALL `
xxx代表用户名 
3、然后保存退出，再次切换到普通用户下，执行sudo命令就可以看到相应的信息了。 