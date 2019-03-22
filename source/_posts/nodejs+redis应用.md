---
title: nodejs+redis应用
date: 2017-10-10 11:20:26
tags: [centos7, redis]
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