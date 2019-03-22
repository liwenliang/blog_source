---
title: chown命令
date: 2017-10-09 13:20:26
tags: [linux, cmd]
---
chown将指定文件的拥有者改为指定的用户或组，用户可以是用户名或者用户ID；组可以是组名或者组ID；文件是以空格分开的要改变权限的文件列表，支持通配符。系统管理员经常使用chown命令，在将文件拷贝到另一个用户的名录下之后，让用户拥有使用该文件的权限。 

<!-- more -->

1. 命令格式：

	**chown [选项]... [所有者][:[组]] 文件...**

2. 命令功能：

	通过chown改变文件的拥有者和群组。在更改文件的所有者或所属群组时，可以使用用户名称和用户识别码设置。普通用户不能将自己的文件改变成其他的拥有者。其操作权限一般为管理员。

3. 命令参数：

	必要参数:

　　　　-c 显示更改的部分的信息

　　　　-f 忽略错误信息

　　　　-h 修复符号链接

　　　　**-R 处理指定目录以及其子目录下的所有文件**

　　　　**-v 显示详细的处理信息**

　　　　-deference 作用于符号链接的指向，而不是链接文件本身

　　选择参数:

　　　　--reference=<目录或文件> 把指定的目录/文件作为参考，把操作的文件/目录设置成参考文件/目录相同拥有者和群组

　　　　--from=<当前用户：当前群组> 只有当前用户和群组跟指定的用户和群组相同时才进行改变

　　　　--help 显示帮助信息

　　　　--version 显示版本信息

4．使用实例：

　　**实例1：改变拥有者和群组**

　　命令：

　　chown mail:mail log2012.log
	
　　输出：　　

```
[root@localhost test6]# ll
---xr--r-- 1 root users 302108 11-30 08:39 linklog.log
---xr--r-- 1 root users 302108 11-30 08:39 log2012.log
-rw-r--r-- 1 root users     61 11-30 08:39 log2013.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2014.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2015.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2016.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2017.log
[root@localhost test6]# chown mail:mail log2012.log 
[root@localhost test6]# ll
---xr--r-- 1 root users 302108 11-30 08:39 linklog.log
---xr--r-- 1 mail mail  302108 11-30 08:39 log2012.log
-rw-r--r-- 1 root users     61 11-30 08:39 log2013.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2014.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2015.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2016.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2017.log
[root@localhost test6]#
```
说明：

　　**实例2：改变文件拥有者和群组**

　　命令：

　　　　chown root: log2012.log

　　输出：　　　　

```
[root@localhost test6]# ll
总计 604
---xr--r-- 1 root users 302108 11-30 08:39 linklog.log
---xr--r-- 1 mail mail  302108 11-30 08:39 log2012.log
-rw-r--r-- 1 root users     61 11-30 08:39 log2013.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2014.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2015.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2016.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2017.log
[root@localhost test6]# chown root: log2012.log 
[root@localhost test6]# ll
总计 604
---xr--r-- 1 root users 302108 11-30 08:39 linklog.log
---xr--r-- 1 root root  302108 11-30 08:39 log2012.log
-rw-r--r-- 1 root users     61 11-30 08:39 log2013.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2014.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2015.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2016.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2017.log
[root@localhost test6]#
```
说明：

　　**实例3：改变文件群组**

　　命令：

　　　　chown :mail log2012.log

　　输出：　　

```
[root@localhost test6]# ll
总计 604
---xr--r-- 1 root users 302108 11-30 08:39 linklog.log
---xr--r-- 1 root root  302108 11-30 08:39 log2012.log
-rw-r--r-- 1 root users     61 11-30 08:39 log2013.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2014.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2015.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2016.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2017.log
[root@localhost test6]# chown :mail log2012.log 
[root@localhost test6]# ll
总计 604
---xr--r-- 1 root users 302108 11-30 08:39 linklog.log
---xr--r-- 1 root mail  302108 11-30 08:39 log2012.log
-rw-r--r-- 1 root users     61 11-30 08:39 log2013.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2014.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2015.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2016.log
-rw-r--r-- 1 root users      0 11-30 08:39 log2017.log
```
说明：

　　**实例4：改变指定目录以及其子目录下的所有文件的拥有者和群组 **

　　命令：

　　　　**chown -R *-v* root:mail test6**

　　输出：　　

```
[root@localhost test]# ll
drwxr-xr-x 2 root users   4096 11-30 08:39 test6
[root@localhost test]# chown -R -v root:mail test6
“test6/log2014.log” 的所有者已更改为 root:mail
“test6/linklog.log” 的所有者已更改为 root:mail
“test6/log2015.log” 的所有者已更改为 root:mail
“test6/log2013.log” 的所有者已更改为 root:mail
“test6/log2012.log” 的所有者已保留为 root:mail
“test6/log2017.log” 的所有者已更改为 root:mail
“test6/log2016.log” 的所有者已更改为 root:mail
“test6” 的所有者已更改为 root:mail
[root@localhost test]# ll
drwxr-xr-x 2 root mail   4096 11-30 08:39 test6
[root@localhost test]# cd test6
[root@localhost test6]# ll
总计 604
---xr--r-- 1 root mail 302108 11-30 08:39 linklog.log
---xr--r-- 1 root mail 302108 11-30 08:39 log2012.log
-rw-r--r-- 1 root mail     61 11-30 08:39 log2013.log
-rw-r--r-- 1 root mail      0 11-30 08:39 log2014.log
-rw-r--r-- 1 root mail      0 11-30 08:39 log2015.log
-rw-r--r-- 1 root mail      0 11-30 08:39 log2016.log
-rw-r--r-- 1 root mail      0 11-30 08:39 log2017.log
```
	
说明：