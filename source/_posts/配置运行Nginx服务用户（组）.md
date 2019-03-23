---
title: 配置运行Nginx服务用户（组）
tags:
  - nginx
originContent: |-
  # user 指令
  user user [group]
      user,指定可以运行Nginx服务器的用户
      group，可选项，指定可以运行Nginx服务器的用户组

  <!-- more -->

  只有被设置的用户或者用户组成员才有权限启动Nginx进程，如果是其他用户（test_user)尝试启动Nginx进程，将会报错

  如果希望所有用户都可以启动Nginx进程，有两种办法：一是将此指令行注释掉：
  \#user [user] [group];
  或者将用户（和用户组）设置为nobody：
  user nobody nobody;
  这也是user指令的默认配置。user指令只能在全局块中配置。

  > 注意：在Nginx配置文件中，每一条指令配置都必须以分号结束，不要忘记。
categories:
  - 后端开发
toc: false
date: 2017-08-26 20:16:20
---

# user 指令
user user [group]
    user,指定可以运行Nginx服务器的用户
    group，可选项，指定可以运行Nginx服务器的用户组

<!-- more -->

只有被设置的用户或者用户组成员才有权限启动Nginx进程，如果是其他用户（test_user)尝试启动Nginx进程，将会报错

如果希望所有用户都可以启动Nginx进程，有两种办法：一是将此指令行注释掉：
\#user [user] [group];
或者将用户（和用户组）设置为nobody：
user nobody nobody;
这也是user指令的默认配置。user指令只能在全局块中配置。

> 注意：在Nginx配置文件中，每一条指令配置都必须以分号结束，不要忘记。