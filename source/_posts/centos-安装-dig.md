---
title: centos 安装 dig
tags:
  - linux
categories:
  - 后端开发
toc: false
date: 2018-01-11 14:22:49
---

执行以下命令：

```
yum install bind-utils
```

<!-- more -->

例如：

```
# dig www.le.com

; <<>> DiG 9.9.4-RedHat-9.9.4-51.el7_4.1 <<>> www.le.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 42461
;; flags: qr rd ra; QUERY: 1, ANSWER: 4, AUTHORITY: 5, ADDITIONAL: 6

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;www.le.com.			IN	A

;; ANSWER SECTION:
www.le.com.		        525	    IN	CNAME	letvimg.sf.cdnle.com.
letvimg.sf.cdnle.com.	598	    IN	CNAME	foreign.sf.cdnle.com.
foreign.sf.cdnle.com.	3235	IN	CNAME	vip.sf.cdnle.com.
vip.sf.cdnle.com.	    45	    IN	A	    123.125.39.240

;; AUTHORITY SECTION:
cdnle.com.		171361	IN	NS	ns2.cdnle.com.
cdnle.com.		171361	IN	NS	ns4.cdnle.com.
cdnle.com.		171361	IN	NS	ns1.cdnle.com.
cdnle.com.		171361	IN	NS	ns5.cdnle.com.
cdnle.com.		171361	IN	NS	ns3.cdnle.com.

;; ADDITIONAL SECTION:
ns1.cdnle.com.		171361	IN	A	211.154.225.249
ns2.cdnle.com.		171361	IN	A	123.125.47.222
ns3.cdnle.com.		171361	IN	A	36.110.161.240
ns4.cdnle.com.		171361	IN	A	123.59.122.247
ns5.cdnle.com.		171361	IN	A	36.110.223.236

;; Query time: 25 msec
;; SERVER: 10.0.2.3#53(10.0.2.3)
;; WHEN: 四 1月 11 06:24:03 UTC 2018
;; MSG SIZE  rcvd: 296
```
