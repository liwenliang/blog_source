---
title: 解决在Centos系统中无法切换到jenkins用户
tags:
  - jenkins
categories:
  - Web开发
toc: false
date: 2019-08-14 14:13:48
---

今天在Centos上安装好了jenkins后，由于jenkins会自动创建一个用户，所以我想切换到jenkins用户，模拟执行一些脚本，
我执行下面语句
```bash
su jenkins
```
虽然不报错，但是就是切换不过去。原来/etc/passwd文件中的/bin/bash被yum安装的时候变成了/bin/false，需要修改回去。接着执行sudo vim /etc/passwd命令,把false改为bash

```bash
vim /etc/passwd
```

给jenkins用户添加一个密码
```bash
passwd jenkins
```

 修改完毕后，执行su jenkins命令。结果新的问题又来啦，当我切换到jenkins用户后，命令提示符的用户名不是jenkins而变成了如下：
```bash
-bash-4.1#
```

原因是在安装jenkins时，jenkins只是创建了jenkins用户，并没有为其创建home目录。所以系统就不会在创建用户的时候，自动拷贝/etc/skel目录下的用户环境变量文件到用户家目录，也就导致这些文件不存在，出现-bash-4.1#的问题了
以下命令是在切换到jenkins用户下执行的！（只是用户现在显示的是-bash-4.1）

这个时候呢，参考网上的做法我执行下面步骤：
```bash
vim ~/.bash_profile
```
执行上面的命令，即使没有.bash_profile文件，linux会自动创建。
然后再添加这句

```bash
export PS1='[\u@\h \W]\$'
```

 PS1：命令行提示符环境变量
 

```bash
source ~/.bash_profile
```
