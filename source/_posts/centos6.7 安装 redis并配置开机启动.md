---
title: centos6.7 安装 redis并配置开机启动
tags:
  - redis
originContent: "在实际开发中有时候我们生产环境使用的服务器并不是centos7，这样造成我们安装软件需要使用更加通用的方式。\n\n1. 首先安装wget工具，下载redis包，我们在github下载release包，官网地址目前无法访问\n\t\n\t```\n\tyum install wget -y\n\twget https://github.com/antirez/redis/archive/4.0.2.tar.gz\n\t```\n<!-- more -->\n\n2. 解压&make&安装\n\t```\n\ttar zxvf 4.0.2.tar.gz \n\tcd redis-4.0.2/\n\tmake\n\tcd src && make install\n\t```\n\t![make && make install](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-175039.png)\n\t执行完make install以后，会把redis-cli、redis-server放到/usr/local/bin/下边，由于服务器并没有把这个路径加到环境变量里边，因此我们需要多一步操作\n3. 修改环境变量，使redis命令生效\n\n\t```\n\tcd ~/\n\tvi .bashrc\n\t\n\t# 然后再文件末尾添加如下命令\n\tPATH=$PATH:/usr/local/bin/\n\t\n\t#保存后执行下边命令\n\tsource .bashrc\n\t```\n\t![修改环境变量](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-175531.png)\n4. 复制配置文件到/etc 目录下\n\n\t```\n\tcp ~/redis-4.0.2/redis.conf /etc/\n\t```\n\t\n5. 建立用户与日志目录，建议为Redis单独建立一个用户，并新建data和日志文件夹\n\n\t```\n\tsudo useradd redis  \n\tsudo mkdir -p /var/lib/redis  \n\tsudo mkdir -p /var/log/redis  \n\tsudo chown redis.redis /var/lib/redis  \n\tsudo chown redis.redis /var/log/redis \n\t```\n6. 修改配置文件\n\t\n\t```\n\tvi /etc/redis.conf\n\t```\n\t\n\t修改绑定IP，让本机之外的IP也能够访问redis bind 0.0.0.0\n\t![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-180325.png)\n\t\n\t修改启动模式为后台启动 daemonize yes\n\t![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-180500.png)\n\t\n\t修改数据文件存储位置 dir /var/lib/redis\n\t![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-180751.png)\n\t\n7. 配置init脚本\n\n\t```\n\tvi /etc/init.d/redis\n\t```\n\t\n\t```\n\t# chkconfig:   2345 90 10  \n  \n\t# description:  Redis is a persistent key-value database  \n\t  \n\t###########################  \n\tPATH=/usr/local/bin:/sbin:/usr/bin:/bin  \n\t     \n\tREDISPORT=6379  \n\tEXEC=/usr/local/bin/redis-server  \n\tREDIS_CLI=/usr/local/bin/redis-cli  \n\t     \n\tPIDFILE=/var/run/redis.pid  \n\tCONF=\"/etc/redis.conf\"  \n\t     \n\tcase \"$1\" in  \n\t    start)  \n\t        if [ -f $PIDFILE ]  \n\t        then  \n\t                echo \"$PIDFILE exists, process is already running or crashed\"  \n\t        else  \n\t                echo \"Starting Redis server...\"  \n\t                $EXEC $CONF  \n\t        fi  \n\t        if [ \"$?\"=\"0\" ]   \n\t        then  \n\t              echo \"Redis is running...\"  \n\t        fi  \n\t        ;;  \n\t    stop)  \n\t        if [ ! -f $PIDFILE ]  \n\t        then  \n\t                echo \"$PIDFILE does not exist, process is not running\"  \n\t        else  \n\t                PID=$(cat $PIDFILE)  \n\t                echo \"Stopping ...\"  \n\t                $REDIS_CLI -p $REDISPORT SHUTDOWN  \n\t                while [ -x ${PIDFILE} ]  \n\t               do  \n\t                    echo \"Waiting for Redis to shutdown ...\"  \n\t                    sleep 1  \n\t                done  \n\t                echo \"Redis stopped\"  \n\t        fi  \n\t        ;;  \n\t   restart|force-reload)  \n\t        ${0} stop  \n\t        ${0} start  \n\t        ;;  \n\t  *)  \n\t    echo \"Usage: /etc/init.d/redis {start|stop|restart|force-reload}\" >&2  \n\t        exit 1  \n\tesac  \n\t##############################  \n\t```\n8. 添加执行权限\n\n\t```\n\tchmod +x /etc/init.d/redis  \n\t```\n\t\n9. 设定开机启动服务\n\n\t```\n\tsudo chkconfig redis on    \n\t```\n10. 启动，停止redis\n\t\n\t```\n\tservice redis start    \n\tservice redis stop \n\t```\n\t\n11. 测试redis\n\n\t```\n\tredis-cli\n\t```\n12. 通过rdm连接\n\t\n\t![通过rdm连接](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-183804@2x.png)"
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