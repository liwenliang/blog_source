---
title: Git仓库完全迁移，包括所有的分支和标签，当然也包括日志
tags:
  - git迁移
categories:
  - 项目总结
toc: false
date: 2018-07-11 17:27:57
---

度娘了一堆git仓库迁移的内容，一个个都比较麻烦，而且本地下了代码，还要删去库地址，再切换到新库的地址上传。

一般这种操作都只是master分支，其他分支还要一个一个来，后来在51CTO上找了一个文章，简单明了，一下就全搞定了。

包括所有的分支、标签、日志，一个不少。

当然账号对应的事就没办法了。

四行命令：

```
git clone --mirror <URL to my OLD repo location>
cd <New directory where your OLD repo was cloned>
git remote set-url origin <URL to my NEW repo location>
git push -f origin
```
