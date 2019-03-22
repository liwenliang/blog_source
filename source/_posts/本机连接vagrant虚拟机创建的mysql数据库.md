---
title: 本机连接vagrant虚拟机创建的mysql数据库
date: 2018-02-25 12:30:38
tags: [mysql, vagrant]
---

当学习数据库的时候经常会用到虚拟机，以避免把自己的机器当成测试机搞来搞去，这里我用的是vagrant，用起来比较方便，
我在vagrant里边创建了一个数据库，希望通过本机的可视化工具连接上，直接连接发现会报错：

```
Host '192.168.33.1' is not allowed to connect to this MariaDB server
```

<!-- more -->

这应该是数据库本身对外部访问的一个限制，现在我们处理一下：

```
CREATE USER 'kivi'@'localhost' IDENTIFIED BY 'kivi';

GRANT ALL PRIVILEGES ON *.* TO 'kivi'@'localhost' WITH GRANT OPTION;

CREATE USER 'kivi'@'%' IDENTIFIED BY 'kivi';

GRANT ALL PRIVILEGES ON *.* TO 'kivi'@'%' WITH GRANT OPTION;
```

好了，在虚拟机的数据库环境执行完这四条命令以后，我们在使用工具连接，用户名kivi，密码kivi；

成功连接！