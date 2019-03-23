---
title: centos 安装 dig
tags:
  - linux
originContent: "执行以下命令：\n\n```\nyum install bind-utils\n```\n\n<!-- more -->\n\n例如：\n\n```\n# dig www.le.com\n\n; <<>> DiG 9.9.4-RedHat-9.9.4-51.el7_4.1 <<>> www.le.com\n;; global options: +cmd\n;; Got answer:\n;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 42461\n;; flags: qr rd ra; QUERY: 1, ANSWER: 4, AUTHORITY: 5, ADDITIONAL: 6\n\n;; OPT PSEUDOSECTION:\n; EDNS: version: 0, flags:; udp: 4096\n;; QUESTION SECTION:\n;www.le.com.\t\t\tIN\tA\n\n;; ANSWER SECTION:\nwww.le.com.\t\t        525\t    IN\tCNAME\tletvimg.sf.cdnle.com.\nletvimg.sf.cdnle.com.\t598\t    IN\tCNAME\tforeign.sf.cdnle.com.\nforeign.sf.cdnle.com.\t3235\tIN\tCNAME\tvip.sf.cdnle.com.\nvip.sf.cdnle.com.\t    45\t    IN\tA\t    123.125.39.240\n\n;; AUTHORITY SECTION:\ncdnle.com.\t\t171361\tIN\tNS\tns2.cdnle.com.\ncdnle.com.\t\t171361\tIN\tNS\tns4.cdnle.com.\ncdnle.com.\t\t171361\tIN\tNS\tns1.cdnle.com.\ncdnle.com.\t\t171361\tIN\tNS\tns5.cdnle.com.\ncdnle.com.\t\t171361\tIN\tNS\tns3.cdnle.com.\n\n;; ADDITIONAL SECTION:\nns1.cdnle.com.\t\t171361\tIN\tA\t211.154.225.249\nns2.cdnle.com.\t\t171361\tIN\tA\t123.125.47.222\nns3.cdnle.com.\t\t171361\tIN\tA\t36.110.161.240\nns4.cdnle.com.\t\t171361\tIN\tA\t123.59.122.247\nns5.cdnle.com.\t\t171361\tIN\tA\t36.110.223.236\n\n;; Query time: 25 msec\n;; SERVER: 10.0.2.3#53(10.0.2.3)\n;; WHEN: 四 1月 11 06:24:03 UTC 2018\n;; MSG SIZE  rcvd: 296\n```"
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