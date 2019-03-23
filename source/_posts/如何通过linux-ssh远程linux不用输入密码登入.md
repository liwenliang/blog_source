---
title: 如何通过linux ssh远程linux不用输入密码登入
tags:
  - linux
originContent: >-
  1. 首先登入一台linux服务器，此台做为母机（即登入其他linux系统用这台做为入口）；执行一行命令生成key文件：


  ```

  ssh-keygen -t rsa

  ```


  2. 在母机上，进入/root/.ssh目录，找到id_rsa.pub该文件，这个文件就是刚才执行ssh-keygen所生成的公钥key文件。


  ```

  cd ~/.ssh/

  ls -lsh

  ```


  3. 用scp命令，将母机产生的key拷一份到远程的linux服务器上，并命名成authorized_keys；这一步的操作需要手动输入密码。


  ```

  scp ~/.ssh/id_rsa.pub  root@192.168.1.113:/root/.ssh/authorized_keys

  ```


  4. 现在为止，你已完成了所有的操作；可在母机通过ssh root@192.168.1.113
  你会发现不在用输放密码了。相同的scp命令也是一样的情况，无需手动输入密码。
categories:
  - 后端开发
toc: false
date: 2018-04-08 13:52:41
---

1. 首先登入一台linux服务器，此台做为母机（即登入其他linux系统用这台做为入口）；执行一行命令生成key文件：

```
ssh-keygen -t rsa
```

2. 在母机上，进入/root/.ssh目录，找到id_rsa.pub该文件，这个文件就是刚才执行ssh-keygen所生成的公钥key文件。

```
cd ~/.ssh/
ls -lsh
```

3. 用scp命令，将母机产生的key拷一份到远程的linux服务器上，并命名成authorized_keys；这一步的操作需要手动输入密码。

```
scp ~/.ssh/id_rsa.pub  root@192.168.1.113:/root/.ssh/authorized_keys
```

4. 现在为止，你已完成了所有的操作；可在母机通过ssh root@192.168.1.113 你会发现不在用输放密码了。相同的scp命令也是一样的情况，无需手动输入密码。