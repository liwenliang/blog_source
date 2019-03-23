---
title: nginx设置cookie点滴感悟
tags:
  - nginx
originContent: >-
  公司有一个简单需求：利用nginx的ngx_http_auth_request_module模块设计一个鉴权接口，将鉴权接口返回的字符串赋值给$trueValue，设置到cookie的Value中。


  面对这个简单需求，首先，去百度搜索nginx、cookie等相关的关键字，有价值的内容不多，而且写的不够详细，令自己踩了不少坑，所以萌生了写作本文的想法。


  搜索后，我首先利用百度知道 https://zhidao.baidu.com/question/519658630466299605.html
  这篇文章，能成功设置cookie。本地浏览器向nginx发送一个get/post请求，利用chrome开发者工具查看应答报文的头部已包含set_cookie字段，同时，在Resources标签可见已写入库，如下图1所示。但是，仅仅按照这文章这么做的话，Domain、Expires字段均为默认值，显然这不符合最终要求。


  接下来，继续搜索找到 http://www.iteye.com/problems/60159
  这篇文章，文中在上篇文章的基础上，设置了自定义的domain、expires等字段。但是，这文章写得太简略了，根本没讲清楚使用原则是什么，无法让人彻底明白每个key应该如何设置，这为我的使用埋下一个坑。当我使用时，原封不动粘贴上面这个链接的add_header指令内容到自己nginx.conf中。经测试，在应答报文中的确包含了set-cookie字段（我们知道，浏览器获知了应答报文的set-cookie字段的信息，即可在本地保留cookie），但是没有入库(即在图1中查询不到任何信息)。


  所以就继续搜索，想解决没有入库的问题。此时，我这边思路出现了问题，错误地做出了如下判断：


  1.
  认为使用add_header指令时，我的字段格式有问题，希望通过将如下指令的双引号换成单引号、增加或者删除空格、更改时间格式等方式让cookie入库，结果折腾好长时间宣告失败。


  ```

  add_header "Set-Cookie" "rec_id=$rec_id;domain=.test1.com;path=/;expires=Mon,
  29-Jun-2015 06:11:17 GMT";

  ```


  2. 认为本地chrome浏览器的设置有问题，遂尝试换成IE、Firefox等其他浏览器，看一下能否正常入库，经实验后，均无法正常入库。

  3. 认为add_header指令本身的问题，遂尝试ngx_http_userid_module这个专门用于设置cookie的nginx模块。


  ngx_http_userid_module模块有专门的语句设置path、expires、domain等字段。但是，我通篇阅读了有关这个模块的nginx官网(
  http://nginx.org/en/docs/http/ngx_http_userid_module.html )文档后，竟然没有找到设置cookie
  Value的方式。以前也遇到过这种官方文档写作不明确的情况，比如：post_action指令在nginx官方文档找不到任何说明，但是实际上是可以使用的。没办法，只能自己尝试。我感觉既然userid_name指令是用来设置cookie
  name的，那么如果我用userid_name uid="myCookieValue"这样，能不能成功呢？实验后，发现的确可以将cookie
  name设置成uid，cookie Value设置成"myCookieValue"。


  本以为这样就能完成任务了，坑爹的事儿又来了。如果这么用的话，竟然不支持nginx变量！比如，我设置userid_name
  uid=$trueValue，其中的$trueValue在nginx.conf中已赋值，但浏览器访问nginx后获取的Value值，竟然是$trueValue，而不是$trueValue中存储的值！这就尴尬了，这条路又走不通了。


  走了这么多弯路后，又开始大量搜索相关资料。终于 https://segmentfault.com/q/1010000006116583
  中给出了"同源原则"，这简单的四字提示。我突然想到我的add_header指令中的domain字段设置的是.test1.com，而nginx的虚拟主机配置的域名是www.abc.com，这两者是不一致的，很有可能根据同源原则，只有这两者一致后，cookie才能设置成功。最后，经过实验，证明了我的判断是正确的。


  好了，文章抄到这里作为备忘，自己的设置如下：


  ```

  server {
          listen                  80;
          server_name             xxx.xx.com;
          charset                 utf-8;
          add_header Set-Cookie   'client_ip=$proxy_add_x_forwarded_for;domain=xxx.xx.com;path=/;';

          index index.html;
          root  /letv/www;
  }

  ```
categories:
  - 后端开发
toc: false
date: 2018-04-11 13:54:55
---

公司有一个简单需求：利用nginx的ngx_http_auth_request_module模块设计一个鉴权接口，将鉴权接口返回的字符串赋值给$trueValue，设置到cookie的Value中。

面对这个简单需求，首先，去百度搜索nginx、cookie等相关的关键字，有价值的内容不多，而且写的不够详细，令自己踩了不少坑，所以萌生了写作本文的想法。

搜索后，我首先利用百度知道 https://zhidao.baidu.com/question/519658630466299605.html 这篇文章，能成功设置cookie。本地浏览器向nginx发送一个get/post请求，利用chrome开发者工具查看应答报文的头部已包含set_cookie字段，同时，在Resources标签可见已写入库，如下图1所示。但是，仅仅按照这文章这么做的话，Domain、Expires字段均为默认值，显然这不符合最终要求。

接下来，继续搜索找到 http://www.iteye.com/problems/60159 这篇文章，文中在上篇文章的基础上，设置了自定义的domain、expires等字段。但是，这文章写得太简略了，根本没讲清楚使用原则是什么，无法让人彻底明白每个key应该如何设置，这为我的使用埋下一个坑。当我使用时，原封不动粘贴上面这个链接的add_header指令内容到自己nginx.conf中。经测试，在应答报文中的确包含了set-cookie字段（我们知道，浏览器获知了应答报文的set-cookie字段的信息，即可在本地保留cookie），但是没有入库(即在图1中查询不到任何信息)。

所以就继续搜索，想解决没有入库的问题。此时，我这边思路出现了问题，错误地做出了如下判断：

1. 认为使用add_header指令时，我的字段格式有问题，希望通过将如下指令的双引号换成单引号、增加或者删除空格、更改时间格式等方式让cookie入库，结果折腾好长时间宣告失败。

```
add_header "Set-Cookie" "rec_id=$rec_id;domain=.test1.com;path=/;expires=Mon, 29-Jun-2015 06:11:17 GMT";
```

2. 认为本地chrome浏览器的设置有问题，遂尝试换成IE、Firefox等其他浏览器，看一下能否正常入库，经实验后，均无法正常入库。
3. 认为add_header指令本身的问题，遂尝试ngx_http_userid_module这个专门用于设置cookie的nginx模块。

ngx_http_userid_module模块有专门的语句设置path、expires、domain等字段。但是，我通篇阅读了有关这个模块的nginx官网( http://nginx.org/en/docs/http/ngx_http_userid_module.html )文档后，竟然没有找到设置cookie Value的方式。以前也遇到过这种官方文档写作不明确的情况，比如：post_action指令在nginx官方文档找不到任何说明，但是实际上是可以使用的。没办法，只能自己尝试。我感觉既然userid_name指令是用来设置cookie name的，那么如果我用userid_name uid="myCookieValue"这样，能不能成功呢？实验后，发现的确可以将cookie name设置成uid，cookie Value设置成"myCookieValue"。

本以为这样就能完成任务了，坑爹的事儿又来了。如果这么用的话，竟然不支持nginx变量！比如，我设置userid_name uid=$trueValue，其中的$trueValue在nginx.conf中已赋值，但浏览器访问nginx后获取的Value值，竟然是$trueValue，而不是$trueValue中存储的值！这就尴尬了，这条路又走不通了。

走了这么多弯路后，又开始大量搜索相关资料。终于 https://segmentfault.com/q/1010000006116583 中给出了"同源原则"，这简单的四字提示。我突然想到我的add_header指令中的domain字段设置的是.test1.com，而nginx的虚拟主机配置的域名是www.abc.com，这两者是不一致的，很有可能根据同源原则，只有这两者一致后，cookie才能设置成功。最后，经过实验，证明了我的判断是正确的。

好了，文章抄到这里作为备忘，自己的设置如下：

```
server {
        listen                  80;
        server_name             xxx.xx.com;
        charset                 utf-8;
        add_header Set-Cookie   'client_ip=$proxy_add_x_forwarded_for;domain=xxx.xx.com;path=/;';

        index index.html;
        root  /letv/www;
}
```