---
title: centos 6 安装n来控制node版本
tags:
  - nodejs
categories:
  - NodeJS
toc: false
date: 2018-04-02 18:03:35
---

centos6发现无法安装nvm，原因：https://github.com/creationix/nvm/issues/1661

解决办法没有给出，于是换成使用n来安装node版本

自己找到一种办法，就是将已经安装过nvm的机器上压缩.nvm文件夹，然后上传不能安装的到服务器上,解压以后再在.bash_profile里边增加如下代码

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

删除调.nvm里边的version/node下的版本，否则会有一些问题

执行下边代码让设置生效：

```
source .bash_profile
```

n需要通过nvm安装，因此先试用centos yum工具来安装一个默认版本node和npm

```
yum install nodejs -y
yum install npm -y
```

<!-- more -->

完了以后可以看到node版本很低，不满足我们的需求，于是使用n来升级node版本

1、首先通过npm安装node的版本管理工具“n“，不用惊讶，名字就是这么简单，就叫n。据了解，n是node下的一个模块，作者是Express框架的开发者。

```
npm i -g n
```

2、检查n模块

先查看系统node的安装路径，n模块的默认路径为 ‘/usr/local’。

```
$ which node
```
```
/data/home/server/nodejs/bin/node   #举个例子
```

如果路径与n模块的默认路径相同可以跳过3步骤。

3、通过N_PREFIX变量来修改 n 的默认node安装路径。

(1) 编辑环境配置文件

```
vim ~/.bash_profile
```

(2) 将下面两行代码插入到文件末尾

```
export N_PREFIX=/data/home/server/nodejs #node实际安装位置
export PATH=$N_PREFIX/bin:$PATH
```

(3) :wq保存退出；

执行source使修改生效。

```
$ source ~/.bash_profile
```

(4) 确认一下环境变量是否生效。

```
echo $N_PREFIX
/data/home/server/nodejs
```

4、n模块常用命令

```
Commands:
  n                              Output versions installed
  n latest                       Install or activate the latest node release
  n -a x86 latest                As above but force 32 bit architecture
  n stable                       Install or activate the latest stable node release
  n lts                          Install or activate the latest LTS node release
  n <version>                    Install node <version>
  n use <version> [args ...]     Execute node <version> with [args ...]
  n bin <version>                Output bin path for <version>
  n rm <version ...>             Remove the given version(s)
  n prune                        Remove all versions except the current version
  n --latest                     Output the latest node version available
  n --stable                     Output the latest stable node version available
  n --lts                        Output the latest LTS node version available
  n ls                           Output the versions of node available
```

(1) 安装node最新版本

```
n latest
```

(2) 安装稳定版

```
n stable
```

(3) 安装指定版本

```
n v9.5.0
```

(4) 查看已安装版本

```
n
```
(5) 删除指定版本

```
n rm 6.4.0
```
