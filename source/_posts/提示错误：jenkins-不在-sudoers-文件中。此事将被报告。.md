---
title: 提示错误：jenkins 不在 sudoers 文件中。此事将被报告。
tags:
  - jenkins
originContent: ''
categories:
  - Web开发
toc: false
date: 2019-08-14 14:14:19
---

说明本地服务器jenkins没有root的部分执行权限：
#su    
#cd /etc    
#chmod 740 sudoers    
#vim sudoers 
末尾加上
jenkins   ALL=(root) ALL, !/usr/bin/passwd [A-Za-z]*, !/usr/bin/passwd root  