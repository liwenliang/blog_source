---
title: linux查看端口占用情况
tags:
  - linux
originContent: >-
  1. 查看5150端口占用的命令行如下：


  ```

  lsof -i:5150

  ```


  不识别lsof，则执行`yum install lsof -y`


  结果如下:


  ```

  COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME

  node    6055 root   19u  IPv6  29137      0t0  TCP *:atmp (LISTEN)

  node    6055 root   21u  IPv6  29139      0t0  TCP
  localhost.localdomain:atmp->192.168.34.11:59258 (ESTABLISHED)

  ```


  2. 如果想杀掉node进程则使用


  ```

  sudo kill 6055

  ```
categories:
  - 后端开发
toc: false
date: 2018-04-08 15:25:10
---

1. 查看5150端口占用的命令行如下：

```
lsof -i:5150
```

不识别lsof，则执行`yum install lsof -y`

结果如下:

```
COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
node    6055 root   19u  IPv6  29137      0t0  TCP *:atmp (LISTEN)
node    6055 root   21u  IPv6  29139      0t0  TCP localhost.localdomain:atmp->192.168.34.11:59258 (ESTABLISHED)
```

2. 如果想杀掉node进程则使用

```
sudo kill 6055
```