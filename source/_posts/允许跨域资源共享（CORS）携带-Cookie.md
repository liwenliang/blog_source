---
title: 允许跨域资源共享（CORS）携带 Cookie
tags:
  - Web开发
originContent: ''
categories:
  - Web开发
toc: false
date: 2022-04-02 11:33:32
---

CORS 是一个 W3C 标准，全称是“跨域资源共享”（Cross-origin resource sharing）。

默认浏览器为了安全，遵循“同源策略”，不允许 Ajax 跨域访问资源，而为了允许这种操作，服务器端和客户端都要遵循一些约定。

服务器端需设置以下响应头：

```
Access-Control-Allow-Origin: <origin> | * // 授权的访问源
Access-Control-Max-Age: <delta-seconds> // 预检授权的有效期，单位：秒
Access-Control-Allow-Credentials: true | false // 是否允许携带 Cookie
Access-Control-Allow-Methods: <method>[, <method>]* // 允许的请求动词
Access-Control-Allow-Headers: <field-name>[, <field-name>]* // 额外允许携带的请求头
Access-Control-Expose-Headers: <field-name>[, <field-name>]* // 额外允许访问的响应头
```
我们看到，Access-Control-Allow-Credentials 响应头会使浏览器允许在 Ajax 访问时携带 Cookie，但我们仍然需要对 XMLHttpRequest 设置其 withCredentials 参数，才能实现携带 Cookie 的目标。

示例代码如下：

```javascript
var xhr = new XMLHttpRequest();
xhr.withCredentials = true;
```

注意，为了安全，标准里不允许 Access-Control-Allow-Origin: *，必须指定明确的、与请求网页一致的域名。同时，Cookie 依然遵循“同源策略”，只有用目标服务器域名设置的 Cookie 才会上传，而且使用 document.cookie 也无法读取目标服务器域名下的 Cookie。

下面简单普及一下 CORS 的有关知识。

浏览器对待 CORS 有两种规则，一种只有一次请求，一种要多发送一次“预检查”请求。

简单 CORS
同时满足以下条件：

HEAD
GET
POST
请求头限制，只允许出现：

Accept
Accept-Language
Content-Language
Last-Event-ID
Content-Type 且只允许是：
application/x-www-form-urlencoded
multipart/form-data
text/plain
简单请求浏览器会直接发送该请求，并附加 Origin 头，比如：

```
Origin: localhost:8080
```
服务器会返回：

```
Access-Control-Allow-Origin: http://localhost:8080
Access-Control-Max-Age: 600
Access-Control-Allow-Credentials: true
Access-Control-Expose-Headers: X-Custom-Header
```
浏览器在收到服务器返回时，先检查是否允许访问，不允许则直接报错（可用 XMLHttpRequest.onerror 捕获）。

带预检查的 CORS
不满足简单 CORS 要求的请求，在正式请求前，浏览器会发送一次 OPTIONS 动词的请求。

例如欲请求 PUT /xxx，想额外发送 X-Custom-Header 头，则会先发送：

```
OPTIONS /xxx HTTP/1.1
Origin: http://api.bob.com
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: X-Custom-Header
Host: localhost:8080
```
服务器返回：

```
HTTP/1.1 200 OK
Access-Control-Allow-Credentials: true
Access-Control-Allow-Headers: X-Custom-Header
Access-Control-Allow-Methods: PUT
Access-Control-Allow-Origin: http://localhost:8080
Access-Control-Max-Age:600
Allow: GET, HEAD, POST, PUT, DELETE, TRACE, OPTIONS, PATCH
Vary: Origin
```
浏览器检查完，一切顺利，才发送真正的请求。