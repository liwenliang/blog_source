---
title: pomelo之rpc坑
date: 2018-03-12 16:15:05
tags: [pomelo, rpc]
---

在使用pomelo的RPC功能时，由于不清楚它的尿性，最后的回调没有传入，导致一个问题

服务器上总是有

```
rpc request timeout
rpc callback timeout
```

的错误输出。但是却不影响业务，于是去代码里面搜索

在mailbox那个文件中有这个输出

原因是在每个rpc调用的时候会设置一个callback的timeout。

若是在这个timeout内没有调用callback，则会产生如上输出

后来果断在rpc中将callback传入，整个世界清静了。

另外说一句，官方的chat示例中就有这个错误的示例。它在调用kick的时候callback传入的Null，不出意外的，一会以后它的输出也是rpc timeout….

[原文地址](http://blog.csdn.net/nynyvkhhiiii/article/details/49757095)



