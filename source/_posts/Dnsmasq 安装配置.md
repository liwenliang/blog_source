---
title: Dnsmasq 安装配置
tags:
  - 运维
originContent: >-
  Dnsmasq  官网
  [http://www.thekelleys.org.uk/dnsmasq/doc.html](http://www.thekelleys.org.uk/dnsmasq/doc.html)


  Unbuntu 安装 Dnsmasq 参见
  [https://help.ubuntu.com/community/Dnsmasq](https://help.ubuntu.com/community/Dnsmasq)

  <!-- more -->

  ## 介绍

  DNSmasq 提供两种服务，每种服务可以单独运行：


  1. DNS 服务

  2. DHCP 服务


  本地 DNS 缓存可以加速网络访问，因为用户的浏览器不需要对先前查询过域名再次向 DNS服务器查询。


  DNCP 允许用户的电脑为同一个无线或有限环境中的其他计算机分配 IP 地址。


  ## 安装


  ```

  $ sudo apt-get install dnsmasq 

  ```


  ## 配置

  dnsmasq 配置文件地址


  ```

  $ /etc/dnsmasq.conf

  ```


  本地 DNS 缓存设置


  修改


  ```

  #listen-address=

  ```


  成


  ```

  listen-address=127.0.0.1

  ```


  接下来修改 /etc/resolv.conf，确保新设置的 DNS 服务器 127.0.0.1 出现在 DNS
  服务器列表的顶端。无论何时用户的电脑需要解析一个域名，它会首先请求 dnsmasq （其跑在 127.0.0.1） 


  ```

  cat  /etc/resolv.conf

  search yourisp.com

  nameserver 127.0.0.1

  nameserver 192.168.0.1

  nameserver 205.171.3.25

  nameserver 205.171.3.26

  ```


  修改后配置后重启服务生效


  ```

  $ sudo /etc/init.d/dnsmasq restart

  ```


  [原文地址](http://www.cnblogs.com/DillGao/p/7372014.html)
categories:
  - 后端开发
toc: false
date: 2018-01-10 17:17:10
---

Dnsmasq  官网 [http://www.thekelleys.org.uk/dnsmasq/doc.html](http://www.thekelleys.org.uk/dnsmasq/doc.html)

Unbuntu 安装 Dnsmasq 参见 [https://help.ubuntu.com/community/Dnsmasq](https://help.ubuntu.com/community/Dnsmasq)
<!-- more -->
## 介绍
DNSmasq 提供两种服务，每种服务可以单独运行：

1. DNS 服务
2. DHCP 服务

本地 DNS 缓存可以加速网络访问，因为用户的浏览器不需要对先前查询过域名再次向 DNS服务器查询。

DNCP 允许用户的电脑为同一个无线或有限环境中的其他计算机分配 IP 地址。

## 安装

```
$ sudo apt-get install dnsmasq 
```

## 配置
dnsmasq 配置文件地址

```
$ /etc/dnsmasq.conf
```

本地 DNS 缓存设置

修改

```
#listen-address=
```

成

```
listen-address=127.0.0.1
```

接下来修改 /etc/resolv.conf，确保新设置的 DNS 服务器 127.0.0.1 出现在 DNS 服务器列表的顶端。无论何时用户的电脑需要解析一个域名，它会首先请求 dnsmasq （其跑在 127.0.0.1） 

```
cat  /etc/resolv.conf
search yourisp.com
nameserver 127.0.0.1
nameserver 192.168.0.1
nameserver 205.171.3.25
nameserver 205.171.3.26
```

修改后配置后重启服务生效

```
$ sudo /etc/init.d/dnsmasq restart
```

[原文地址](http://www.cnblogs.com/DillGao/p/7372014.html)