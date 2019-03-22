---
title: centos7 安装 redis并配置开机启动
date: 2017-10-10 14:20:26
tags: [centos7, redis]
---

由于centos7系统已经出现很久，所以优先使用centos7来安装redis，这里做个记录。这里我使用的centos7是通过vagrant创建的一个虚拟机，能够让我最大限度的还原真实服务器环境。

<!-- more -->

1. 首先安装wget工具，下载自动更新ius包文件

	```
	yum install wget -y
	wget -O setup.sh setup.ius.io
	sh ./setup.sh
	```
	
	[https://ius.io/GettingStarted/](https://ius.io/GettingStarted/)这个网站有几种更新yum包的方式，我们这里使用的是自动安装方式，命令也比手动的方式要简单好记，实际工作中我们记住切实可行的就行了。

2. 安装完成以后执行
	
	```
	yum install redis -y
	```
3. 启动redis并设置开机启动redis

	```
	systemctl start redis
	systemctl enable redis
	```
4. 设置除了本机可访问，其他机器也可以访问
	
	```
	whereis redis
	```
	通过这个命令获取redis配置文件地址：
	![redis配置文件地址](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-171857.png)
	然后修改这个文件中NETWORK注释下的bind 127.0.0.1为bind 0.0.0.0
	![修改配置文件](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-172037.png)
5. 重启redis

	```
	systemctl restart redis
	```
6. 通过rdm连接服务器redis
	![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-172353.png)
7. 本机连接redis

	```
	redis-cli
	```

后记：在centos7上安装软件总是那么顺手，但是在我们的生产环境由于历史原因服务器也许不是centos7，这时候安装就变得异常啰嗦与复杂，当然也许是由于我个人用的比较少，觉得麻烦而已。在centos6.7上我也尝试安装了一次，记录如下。
	