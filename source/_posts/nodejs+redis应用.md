---
title: nodejs+redis应用
tags:
  - redis
originContent: "1. 什么是Redis\n\n\tRedis是一个基于BSD开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API.\n\n\t<!-- more -->\n\t\n\t1. Redis存储的是一个个的键值对\n\t2. 通常用Redis做缓存数据库\n\t3. Redis的五种数据类型(字符串，哈希，链表，无序集合，有序集合)\n\n2. Redis安装\n\n\tRedis官方不支持windows安装，所以我们这里以centos7 为例：\n\t\n\t```\n\t$ rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm\n\t$ rpm -Uvh https://centos7.iuscommunity.org/ius-release.rpm\n\t$ yum install redis -y\n\t$ systemctl start redis\n\t$ systemctl enable redis\n\t$ systemctl status redis\n\t```\n\n3. Redis运行\n\tRedis服务器默认使用6379端口\n\t\n\t> resis-server\n\t\n\t也可以自己指定端口\n\t\n\t> redis-server --port 6380\n\t\n\t客户端通过\n\t\n\t> redis-cli\n\t\n\t来连接数据库服务器\n\t\n\t也可以指定服务器地址和端口\n\t\n\t> redis-cli -h 127.0.0.1 -p 6380\n\t\n\t测试客户端和服务器是否连通\n\t\n\t> PING\n\t\n\t返回PONG说明连通了\n\n\t![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/QQ20171010-140354.png)\n\n4. Redis的基本使用\n\n\t4.1 字符串\n\t\n\tset key value 设置key的value\n\t\n\tget key 得到key的value\n\t\n\tincr key key如果是整型自增1\n\t\n\tkeys * 得到所有key\n\t\n\t![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/QQ20171010-140737.png)\n\n\t4.2 哈希\n\t\n\thset key filed value 设置key对象的field属性的value\n\t\n\thget key filed 得到key对象的field属性的value\n\t\n\thgetall key 得到key对象的所有的属性和值\n\n\t![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-141134.png)\n\n\t4.3 链表\n\t\n\t适合存储社交网站的新鲜事\n\t\n\tlpush key value [value ...] 向链表key左边添加元素\n\t\n\trpush key value [value...] 向链表key右边添加元素\n\t\n\tlpop key 移除key链表左边第一个元素\n\t\n\trpop key 移除key链表右边第一元素\n\t\n\tlrange key start stop 获取链表中某一段，包含最大值，-1表示最后一个元素\n\t![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-141515.png)\n\n\t4.4 集合类型\n\t\n\t适合存储文章的标签，因为是唯一的\n\t\n\tsadd key member [member ...] 向集合key中添加元素，如果元素已经存在则忽略\n\t\n\tsrem key member [member ...] 从集合key中删除元素\n\t\n\tsmembers key 返回集合key中所有的元素\n\t\n\tsinter key [key ...] 多个集合执行交集运算\n\t\n\tsdiff key [key ...] 多个集合执行差集运算\n\t\n\tsunion key [key ...] 多个集合执行并集运算\n\t![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-141834.png)\n\n\t4.5 有序集合\n\t\n\t适用于通过文章访问量排序\n\t\n\tzadd key score member [score member ...] 向有序集合key中加入一个或多个元素和分数，如果元素已经存在，则替换分数\n\t\n\tzrem key member [member ...] 删除集合中一个或多个元素\n\t\n\tzrange key start stop [withscores] 按元素分数从小到大顺序返回元素，如需获得对应元素的分数，在尾部加上withscores\n\t\n\tzrevrange key start stop [withscores] 按元素分数从大到小顺序返回元素\n\t![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-142158.png)\n\n5.Redis 数据库桌面管理工具\n\n下载地址:http://redisdesktop.com/download\n\n可以很方便的管理Redis数据库里的键值对\n![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-150845@2x.png)\n\n6.Redis 数据库其他说明\nRedis默认支持16个数据库，编号0~15，每个数据库是独立的不能相互访问，可以通过\n\n> SELECT 数据库编号\n\n来更换数据库\n\nRedis不支持自定义数据库名字\n\nRedis不支持每个数据库设置不同的密码\n\n我们可以通过配置参数database修改支持的数据库个数，当选择的数据库编号超过最大数据库编号，默认使用最大数据库编号的数据库\n\n清空当前数据库的所有内容\n\n> flushdb\n\n清空所有数据库的所有内容\n\n> flushall\n\n随机返回当前数据库的一个键\n\n> randomkey\n\n删除一个或多个key\n\n> del key\n\n返回给定哈希表中所有键的值\n\n> hvals key\n\n**如果需要将redis用作缓存，可以设置生存时间，这样就不会持久化存储**\n\n设置某个键的生存时间\n\n1.(以秒为单位)\n\n> expire key time\n\n2.(以毫秒为单位)\n\n> pexpire key time\n\n以秒为单位返回给定key的剩余生存时间，没有设置生存时间则返回-1\n\n> ttl key\n\n下面我们来看一下怎么样在nodejs中使用redis\n\n1. 安装redis模块\n\n\t> npm install redis\n\n\t通过redis.createClient(port,host,options)来连接redis服务器\n\n```\nvar redis = require(\"redis\"),\n    client = redis.createClient(6380,\"117.121.25.228\",{});\n\nexports.throw = function(bottle,callback){\n    bottle.time = bottle.time || Date.now();\n    var bottleId = Math.random().toString(16);\n    var type = {male:0,female:1};\n    console.log(type[bottle.type]);\n    /*client.SELECT选择数据库编号*/\n    client.SELECT(type[bottle.type],function(){\n        /*client.HMSET 保存哈希键值*/\n        client.HMSET(bottleId,bottle,function(err,result){\n            if(err){\n                return callback({code:0,msg:\"过会儿再来试试吧！\"});\n            }\n            callback({code:1,msg:result});\n            /*设置过期时间为1天*/\n            client.EXPIRE(bottleId,86400);\n        });\n    });\n}\nexports.pick = function(info,callback){\n    var type = {all:Math.round(Math.random()),male:0,female:1};\n    info.type = info.type || 'all';\n    client.SELECT(type[info.type],function(){\n        /*随机返回当前数据库的一个键*/\n        client.RANDOMKEY(function(err,bottleId){\n            if(!bottleId){\n                return callback({code:0,msg:\"大海空空如也...\"});\n            }\n            /*根据key返回哈希对象*/\n            client.HGETALL(bottleId,function(err,bottle){\n                if(err){\n                    return callback({code:0,msg:\"漂流瓶破损了...\"});\n                }\n                callback({code:1,msg:bottle});\n                /*根据key删除键值*/\n                client.DEL(bottleId);\n            });\n        });\n    });\n}\n```\n\n作者：隔壁陈叔叔\n\n链接：[http://www.jianshu.com/p/dbc1da93eae5](http://www.jianshu.com/p/dbc1da93eae5)\n\n來源：简书\n\n著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。"
categories:
  - NodeJS
toc: false
date: 2017-10-10 11:20:26
---

1. 什么是Redis

	Redis是一个基于BSD开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API.

	<!-- more -->
	
	1. Redis存储的是一个个的键值对
	2. 通常用Redis做缓存数据库
	3. Redis的五种数据类型(字符串，哈希，链表，无序集合，有序集合)

2. Redis安装

	Redis官方不支持windows安装，所以我们这里以centos7 为例：
	
	```
	$ rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
	$ rpm -Uvh https://centos7.iuscommunity.org/ius-release.rpm
	$ yum install redis -y
	$ systemctl start redis
	$ systemctl enable redis
	$ systemctl status redis
	```

3. Redis运行
	Redis服务器默认使用6379端口
	
	> resis-server
	
	也可以自己指定端口
	
	> redis-server --port 6380
	
	客户端通过
	
	> redis-cli
	
	来连接数据库服务器
	
	也可以指定服务器地址和端口
	
	> redis-cli -h 127.0.0.1 -p 6380
	
	测试客户端和服务器是否连通
	
	> PING
	
	返回PONG说明连通了

	![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/QQ20171010-140354.png)

4. Redis的基本使用

	4.1 字符串
	
	set key value 设置key的value
	
	get key 得到key的value
	
	incr key key如果是整型自增1
	
	keys * 得到所有key
	
	![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/QQ20171010-140737.png)

	4.2 哈希
	
	hset key filed value 设置key对象的field属性的value
	
	hget key filed 得到key对象的field属性的value
	
	hgetall key 得到key对象的所有的属性和值

	![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-141134.png)

	4.3 链表
	
	适合存储社交网站的新鲜事
	
	lpush key value [value ...] 向链表key左边添加元素
	
	rpush key value [value...] 向链表key右边添加元素
	
	lpop key 移除key链表左边第一个元素
	
	rpop key 移除key链表右边第一元素
	
	lrange key start stop 获取链表中某一段，包含最大值，-1表示最后一个元素
	![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-141515.png)

	4.4 集合类型
	
	适合存储文章的标签，因为是唯一的
	
	sadd key member [member ...] 向集合key中添加元素，如果元素已经存在则忽略
	
	srem key member [member ...] 从集合key中删除元素
	
	smembers key 返回集合key中所有的元素
	
	sinter key [key ...] 多个集合执行交集运算
	
	sdiff key [key ...] 多个集合执行差集运算
	
	sunion key [key ...] 多个集合执行并集运算
	![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-141834.png)

	4.5 有序集合
	
	适用于通过文章访问量排序
	
	zadd key score member [score member ...] 向有序集合key中加入一个或多个元素和分数，如果元素已经存在，则替换分数
	
	zrem key member [member ...] 删除集合中一个或多个元素
	
	zrange key start stop [withscores] 按元素分数从小到大顺序返回元素，如需获得对应元素的分数，在尾部加上withscores
	
	zrevrange key start stop [withscores] 按元素分数从大到小顺序返回元素
	![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-142158.png)

5.Redis 数据库桌面管理工具

下载地址:http://redisdesktop.com/download

可以很方便的管理Redis数据库里的键值对
![](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/blog/QQ20171010-150845@2x.png)

6.Redis 数据库其他说明
Redis默认支持16个数据库，编号0~15，每个数据库是独立的不能相互访问，可以通过

> SELECT 数据库编号

来更换数据库

Redis不支持自定义数据库名字

Redis不支持每个数据库设置不同的密码

我们可以通过配置参数database修改支持的数据库个数，当选择的数据库编号超过最大数据库编号，默认使用最大数据库编号的数据库

清空当前数据库的所有内容

> flushdb

清空所有数据库的所有内容

> flushall

随机返回当前数据库的一个键

> randomkey

删除一个或多个key

> del key

返回给定哈希表中所有键的值

> hvals key

**如果需要将redis用作缓存，可以设置生存时间，这样就不会持久化存储**

设置某个键的生存时间

1.(以秒为单位)

> expire key time

2.(以毫秒为单位)

> pexpire key time

以秒为单位返回给定key的剩余生存时间，没有设置生存时间则返回-1

> ttl key

下面我们来看一下怎么样在nodejs中使用redis

1. 安装redis模块

	> npm install redis

	通过redis.createClient(port,host,options)来连接redis服务器

```
var redis = require("redis"),
    client = redis.createClient(6380,"117.121.25.228",{});

exports.throw = function(bottle,callback){
    bottle.time = bottle.time || Date.now();
    var bottleId = Math.random().toString(16);
    var type = {male:0,female:1};
    console.log(type[bottle.type]);
    /*client.SELECT选择数据库编号*/
    client.SELECT(type[bottle.type],function(){
        /*client.HMSET 保存哈希键值*/
        client.HMSET(bottleId,bottle,function(err,result){
            if(err){
                return callback({code:0,msg:"过会儿再来试试吧！"});
            }
            callback({code:1,msg:result});
            /*设置过期时间为1天*/
            client.EXPIRE(bottleId,86400);
        });
    });
}
exports.pick = function(info,callback){
    var type = {all:Math.round(Math.random()),male:0,female:1};
    info.type = info.type || 'all';
    client.SELECT(type[info.type],function(){
        /*随机返回当前数据库的一个键*/
        client.RANDOMKEY(function(err,bottleId){
            if(!bottleId){
                return callback({code:0,msg:"大海空空如也..."});
            }
            /*根据key返回哈希对象*/
            client.HGETALL(bottleId,function(err,bottle){
                if(err){
                    return callback({code:0,msg:"漂流瓶破损了..."});
                }
                callback({code:1,msg:bottle});
                /*根据key删除键值*/
                client.DEL(bottleId);
            });
        });
    });
}
```

作者：隔壁陈叔叔

链接：[http://www.jianshu.com/p/dbc1da93eae5](http://www.jianshu.com/p/dbc1da93eae5)

來源：简书

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。