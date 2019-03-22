---
title: centos7 安装nvm
date: 2017-12-04 15:03:11
tags: nvm
---

网上找了一些发现这个比较好用：

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.1/install.sh | bash
```

重启命令行，否则不生效，如果用secureCRT则退出再进入即可

下边命令安装稳定版

```
nvm install stable
```

如果是国内环境可以像修改npm一样修改nvm的源：

```
把环境变量 NVM_NODEJS_ORG_MIRROR, 加入到 .bash_profile 文件中:
# nvm
export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node
```