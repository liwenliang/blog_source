---
title: 本地项目切换origin地址
tags:
  - javascript
  - Web开发
  - git迁移
originContent: ''
categories:
  - 项目总结
  - Web开发
toc: false
date: 2022-04-02 11:28:03
---

通过这种方式，我可以同步两个或多个地方的仓库代码，比如我在gitte上和github上同时维护着一个仓库，通过这种方式，我只需要在提交的时候或想到的时候切换下仓库地址，就可以把之前的修改记录同步到另一个仓库

```bash
git remote set-url origin https://xxxx.git
```
好方法记录一下,再加上之前的一篇文章，像代码迁移的操作基本不用慌了：Git仓库完全迁移，包括所有的分支和标签，当然也包括日志

两篇文章都用到了git的set-url方法，所以需要特意看下set-url的作用：

git：更改远程仓库指向 remote url

更改仓库指向：

```bash
git remote set-url origin git@github.com:test/thinkphp.git
```
当然，还有一种方法哦，那就是

```bash
git config -e
```
直接编辑其中origin的url就行了，退出时记得保存

```bash
[core]
        repositoryformatversion = 0
        filemode = false
        bare = false
        logallrefupdates = true
        symlinks = false
        ignorecase = true
        hideDotFiles = dotGitOnly
[remote "origin"]
        fetch = +refs/heads/*:refs/remotes/origin/*
        url = https://github.com/x/thinkphp.git
[branch "master"]
        remote = origin
        merge = refs/heads/master
```
因此我理解这个方法的作用就是，将本地一个仓库，当然包括各种操作和日志记录指向另一个地方，如果新的地方是一个空仓库，则创建同步本地仓库信息，如果新仓库已有仓库，那么应该是会合并记录，当然，如果记录有冲突，估计会出问题，至于出什么问题我也不打算去尝试 😄。