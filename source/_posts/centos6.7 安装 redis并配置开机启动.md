---
title: centos6.7 安装 redis并配置开机启动
tags:
  - redis
categories:
  - NodeJS
toc: false
date: 2017-10-10 18:20:26
---

在实际开发中有时候我们生产环境使用的服务器并不是centos7，这样造成我们安装软件需要使用更加通用的方式。

1. 首先安装wget工具，下载redis包，我们在github下载release包，官网地址目前无法访问
	
	```
	yum install wget -y
	wget https://github.com/antirez/redis/archive/4.0.2.tar.gz
	```
<!-- more -->

2. 解压&make&安装
	```
	tar zxvf 4.0.2.tar.gz 
	cd redis-4.0.2/
	make
	cd src && make install
	```
	![make && make install](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-175039.png)
	执行完make install以后，会把redis-cli、redis-server放到/usr/local/bin/下边，由于服务器并没有把这个路径加到环境变量里边，因此我们需要多一步操作
3. 修改环境变量，使redis命令生效

	```
	cd ~/
	vi .bashrc
	
	# 然后再文件末尾添加如下命令
	PATH=$PATH:/usr/local/bin/
	
	#保存后执行下边命令
	source .bashrc
	```
	![修改环境变量](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-175531.png)
4. 复制配置文件到/etc 目录下

	```
	cp ~/redis-4.0.2/redis.conf /etc/
	```
	
5. 建立用户与日志目录，建议为Redis单独建立一个用户，并新建data和日志文件夹

	```
	sudo useradd redis  
	sudo mkdir -p /var/lib/redis  
	sudo mkdir -p /var/log/redis  
	sudo chown redis.redis /var/lib/redis  
	sudo chown redis.redis /var/log/redis 
	```
6. 修改配置文件
	
	```
	vi /etc/redis.conf
	```
	
	修改绑定IP，让本机之外的IP也能够访问redis bind 0.0.0.0
	![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-180325.png)
	
	修改启动模式为后台启动 daemonize yes
	![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-180500.png)
	
	修改数据文件存储位置 dir /var/lib/redis
	![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-180751.png)
	
7. 配置init脚本

	```
	vi /etc/init.d/redis
	```
	
	```
	# chkconfig:   2345 90 10  
  
	# description:  Redis is a persistent key-value database  
	  
	###########################  
	PATH=/usr/local/bin:/sbin:/usr/bin:/bin  
	     
	REDISPORT=6379  
	EXEC=/usr/local/bin/redis-server  
	REDIS_CLI=/usr/local/bin/redis-cli  
	     
	PIDFILE=/var/run/redis.pid  
	CONF="/etc/redis.conf"  
	     
	case "$1" in  
	    start)  
	        if [ -f $PIDFILE ]  
	        then  
	                echo "$PIDFILE exists, process is already running or crashed"  
	        else  
	                echo "Starting Redis server..."  
	                $EXEC $CONF  
	        fi  
	        if [ "$?"="0" ]   
	        then  
	              echo "Redis is running..."  
	        fi  
	        ;;  
	    stop)  
	        if [ ! -f $PIDFILE ]  
	        then  
	                echo "$PIDFILE does not exist, process is not running"  
	        else  
	                PID=$(cat $PIDFILE)  
	                echo "Stopping ..."  
	                $REDIS_CLI -p $REDISPORT SHUTDOWN  
	                while [ -x ${PIDFILE} ]  
	               do  
	                    echo "Waiting for Redis to shutdown ..."  
	                    sleep 1  
	                done  
	                echo "Redis stopped"  
	        fi  
	        ;;  
	   restart|force-reload)  
	        ${0} stop  
	        ${0} start  
	        ;;  
	  *)  
	    echo "Usage: /etc/init.d/redis {start|stop|restart|force-reload}" >&2  
	        exit 1  
	esac  
	##############################  
	```
8. 添加执行权限

	```
	chmod +x /etc/init.d/redis  
	```
	
9. 设定开机启动服务

	```
	sudo chkconfig redis on    
	```
10. 启动，停止redis
	
	```
	service redis start    
	service redis stop 
	```
	
11. 测试redis

	```
	redis-cli
	```
12. 通过rdm连接
	
	![通过rdm连接](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-183804@2x.png)
