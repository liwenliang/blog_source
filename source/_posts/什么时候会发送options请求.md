---
title: 什么时候会发送options请求
tags:
  - 知识点备忘
originContent: ''
categories:
  - Web开发
toc: false
date: 2020-10-13 15:22:31
---

## 偶然的相遇——options请求
最近写的项目，应用里所有的ajax请求都发送了2遍。由于新项目，基础模块是新搭的，所以出现一些奇葩问题也是意料之中，啊终于第一次在chrome的devTools遇见了活的options请求。
![image.png](http://blogimage.houjiyi.com/FkgRteTQ_wnfq1a29ShCE0Ph4qgu)
### 第1次请求
这里首先发送了一次额外的options请求，在浏览器里看到请求request header 和 response header的信息如下：
（1）预检请求头request header的关键字段：

![image.png](http://blogimage.houjiyi.com/FkKwSEWOU0ceTe7k1j6J983PYvAM)


服务器基于从预检请求头部获得的信息来判断，是否接受接下来的实际请求。

![image.png](http://blogimage.houjiyi.com/FnB1j43wZ3d6dDKgTJsklbG9D09g)

（2）预检响应头response header的关键字段：

![image.png](http://blogimage.houjiyi.com/Fq3-PVNUSp-NMYLDCuP7lPxmiDkb)

此次OPTIONS请求返回了响应头的内容，但没有返回响应实体response body内容。

![image.png](http://blogimage.houjiyi.com/Foxiw5ZheTGsDyWu0LNlRPA6ONBr)

### 第2次请求
这是本来要发送的请求，如图所示是普通的post请求。其中Content-Type的application/json是此次和后端约定的请求内容格式，这个也是后面讲到为什么会发送options请求的原因之一。

![image.png](http://blogimage.houjiyi.com/Fv7OldspEH5iE7AWTWJjW0Sh6Pab)

## 关于OPTIONS请求
从很多资料我们可以了解到使用OPTIONS方法对服务器发起请求，可以检测服务器支持哪些 HTTP 方法。但是这次我们并没有主动去发起OPTIONS请求，那OPTIONS请求为何会自动发起？
### OPTIONS请求自动发起
MDN的CORS一文中提到：

> 规范要求，对那些可能对服务器数据产生副作用的 HTTP 请求方法（特别是 GET 以外的 HTTP 请求，或者搭配某些 MIME 类型的 POST 请求），浏览器必须首先使用 OPTIONS 方法发起一个预检请求（preflight request），从而获知服务端是否允许该跨域请求。

所以这个跨域请求触发了浏览器自动发起OPTIONS请求，看看此次跨域请求具体触发了哪些条件。
### 跨域请求时，OPTIONS请求触发条件
![image.png](http://blogimage.houjiyi.com/FqM0rzrxnL_ZBzFmTgVYgINtVbUj)

由于修改了Content-Type为application/json，触发了CORS预检请求。

## 优化OPTIONS请求：Access-Control-Max-Age 或者 避免触发
可见一旦达到触发条件，跨域请求便会一直发送2次请求，这样增加的请求数是否可优化呢？答案是可以，OPTIONS预检请求的结果可以被缓存。

> Access-Control-Max-Age这个响应首部表示 preflight request  （预检请求）的返回结果（即 Access-Control-Allow-Methods 和Access-Control-Allow-Headers 提供的信息） 可以被缓存的最长时间，单位是秒。(MDN)

如果值为 -1，则表示禁用缓存，每一次请求都需要提供预检请求，即用OPTIONS请求进行检测。
评论区的朋友提醒了，尽量避免不要触发OPTIONS请求，上面例子中把content-type改掉是可以的。在其他场景，比如跨域并且业务有自定义请求头的话就很难避免了。现在使用的axios或者superagent等第三方ajax插件，如果出现CORS预检请求，可以看看默认配置或者二次封装是否规范。

## 总结

OPTIONS请求即预检请求，可用于检测服务器允许的http方法。当发起跨域请求时，由于安全原因，触发一定条件时浏览器会在正式请求之前自动先发起OPTIONS请求，即CORS预检请求，服务器若接受该跨域请求，浏览器才继续发起正式请求。

作者：熊也抱抱
链接：https://juejin.im/post/6844903821634699277
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
