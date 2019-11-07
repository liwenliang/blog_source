---
title: 七牛云存储文件夹上传webpack使用cdn资源地址
tags:
  - javascript
categories:
  - Web开发
toc: false
date: 2019-06-19 17:31:59
---

七牛云存储默认只能单个文件上传，这个用起来很不方便，有时候我的webpack打包后的项目下的dist资源要放在cdn上，这时候需要目录结构都保存，这时候就需要一个客户端能够支持这种上传方法，可以自己写服务，也可以用现成的，在github上找了一个开源客户端，mac、windows都可以用，使用electron、electron-vue开发，如果有问题也可以自己调整下打个包。

<!-- more -->

项目地址：[https://github.com/willnewii/qiniuClient](https://github.com/willnewii/qiniuClient)

打开配置上ak、sk（在七牛个人中心下的密钥管理中）就可以用了，支持文件夹上传。

然后我们看下webpack项目的配置方法：

![image.png](http://blogimage.houjiyi.com/Ft-02F6XdXjjLfsd2WBVVn0augxn)

记录一下，兴许以后会用到，这里有个扩展的地方：既然上传工具支持目录上传，完全可以看下他的上传原理，然后包成自己的服务，这个思路是完全可行的，当后边如果遇到需要自己上传图片的需求的时候，这个东西是可用的。
