---
title: Node.js Sequelize如何实现数据库的读写分离
date: 2018-04-11 12:05:07
tags: [nodejs, sequelize, 数据库, 读写分离]
---
一、前言

在构建高并发的Web应用时，除了应用层要采取负载均衡方案外，数据库也要支持高可用和高并发性。使用较多的数据库优化方案是：通过主从复制(Master-Slave)的方式来同步数据，再通过读写分离(MySQL-Proxy)来提升数据库的并发负载能力。

1. replication选项与读写分离

Sequelize 支持读/写分离，要实现读/写分离可以分别为读和写各创建一个Sequelize实例，更方便的使用方式是在创建实例时，通过replication选项分别指定读/写数据库。

要在Sequelize中使用读/写复制，可以在初始化Sequelize时有时向其replication选项传递一个对象.这个对象read、write两个属性。write是一个单一的对象(即：由单台服务器处理写入)，而read是一个包含对象的数组(即：由多台服务器处理读取)。每台read、write服务器都可以包含以下属性：

· host - 数据库服务器的主机

· port - 数据库服务器的主机端口

· username - 验证用户名

· password - 验证密码

· database - 要连接的数据库

2. Sequelize读/写分离示例

在使用主从复制的多台数据库集群中，可以通过在replication对象的read属性中设置，该属性是一个数组，可以在其中传入一个或多个服务器连接副本。读操作相当于对数据库集群中的从节点进行操作，它会处理所有SELECT查询操作(读操作)。而replication对象的write属性是一个表示服务器连接的对象，写操作相当于主节点，它会处理所有插入、更新、删除操作(写操作)。

```
var sequelize = new Sequelize('database', null, null, {
 dialect: 'mysql',
 port: 3306
 replication: {
  read: [
   { host: '192.168.1.33', username: 'itbilu.com', password: 'pwd' },
   { host: 'localhost', username: 'root', password: null }
  ],
  write: { host: 'localhost', username: 'root', password: null }
 },
 pool: { // 如果需要重写链接池，请在 pool 选项中修改
  maxConnections: 20,
  maxIdleTime: 30000
 },
})
```

我这边的真实代码：

```
import Sequelize from 'sequelize'
import { DB as DBConfig, System as SystemConfig } from '../config'

export default new Sequelize(DBConfig.database, null, null, {
  dialect: SystemConfig.db_type,
  port: DBConfig.port,
  replication: {
    read: [
      {
        host: DBConfig.read.host,
        username: DBConfig.read.username,
        password: DBConfig.read.passwd
      }
    ],
    write: {
      host: DBConfig.write.host,
      username: DBConfig.write.username,
      password: DBConfig.write.passwd
    }
  },
  dialectOptions: { // MySQL > 5.5，其它数据库删除此项
    charset: 'utf8mb4',
    collate: 'utf8mb4_unicode_520_ci',
    supportBigNumbers: true,
    bigNumberStrings: true
  },
  pool: {
    max: 50,
    min: 0,
    idle: 10000
  }
})

```

所有的整体性设置，都会适用于所有节点副本，所以并不需要为每个实例单独指定。在上例中，数据库名和端口号会应用于所有节点副本，用户名和密码选项也同样适用。如果某一节点副本不使用全局设置，则需要在replication选项中单独指定。

注意：Sequelize 并不会设置主从复制节点及节点间的数据同步(复制)，这些操作实际由MySQL(或你所使用的数据库)完成。而 Sequelize 只负责从主从节点写入或读取数据。

Sequelize 会使用连接池来管理节点副本。

默认选项是：

```
{
 maxConnections: 10,
 minConnections: 0,
 maxIdleTime:1000
}
```