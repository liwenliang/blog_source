---
title: vagrant ssh 本机连接方式
tags:
  - vagrant
originContent: |-
  vagrant 启动虚拟机以后想通过本机的命令行进行连接，这时候需要用到openssh

  1. 安装openssh

  ```
  yum install openssh-* -y
  ```

  2. 查看及启动sshd

  ```
  systemctl status sshd
  systemctl restart sshd
  ```

  3. 修改配置文件

  ```
  vi /etc/ssh/sshd_config
  ```

  4. 去掉两处注释

  ```
  Port 22
  ListenAddress 0.0.0.0
  ```

  5. 修改一处配置

  ```
  PasswordAuthentication yes
  ```

  有了这些东西就可以在别的机器上ssh到这个机器了，同时可以使用linux-ssh远程不输入密码
categories:
  - 后端开发
toc: false
date: 2018-04-08 20:27:02
---

vagrant 启动虚拟机以后想通过本机的命令行进行连接，这时候需要用到openssh

1. 安装openssh

```
yum install openssh-* -y
```

2. 查看及启动sshd

```
systemctl status sshd
systemctl restart sshd
```

3. 修改配置文件

```
vi /etc/ssh/sshd_config
```

4. 去掉两处注释

```
Port 22
ListenAddress 0.0.0.0
```

5. 修改一处配置

```
PasswordAuthentication yes
```

有了这些东西就可以在别的机器上ssh到这个机器了，同时可以使用linux-ssh远程不输入密码