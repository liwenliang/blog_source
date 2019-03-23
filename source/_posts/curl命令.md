---
title: curl命令
tags:
  - linux
originContent: "curl命令是一个利用URL规则在命令行下工作的文件传输工具。它支持文件的上传和下载，所以是综合传输工具，但按传统，习惯称curl为下载工具。作为一款强力工具，curl支持包括HTTP、HTTPS、ftp等众多协议，还支持POST、cookies、认证、从指定偏移处下载部分文件、用户代理字符串、限速、文件大小、进度条等特征。做网页处理流程和数据检索自动化，curl可以祝一臂之力。\n\n## 语法\n\n```\ncurl(选项)(参数)\n```\n<!-- more -->\n\n## 选项\n|             选项               |              含义             |\n|-------------------------------|-------------------------------|\n| -a/--append\t                   | 上传文件时，附加到目标文件        |\n|-A/--user-agent \\<string>\t     | 设置用户代理发送给服务器           |\n|-anyauth\t                      |  可以使用“任何”身份验证方法       |\n|-b/--cookie \\<name=string/file> |       cookie字符串或文件读取位置  |\n|    --basic                    |          使用HTTP基本验         |\n|-B/--use-ascii\t               |      使用ASCII /文本传输        |\n|-c/--cookie-jar \\<file>\t     |  操作结束后把cookie写入到这个文件中 |\n|-C/--continue-at \\<offset>|断点续转|\n|-d/--data \\<data>|HTTP POST方式传送数据|\n| --data-ascii \\<data>|以ascii的方式post数据|\n|--data-binary \\<data>|以二进制的方式post数据|\n| --negotiate|使用HTTP身份验证|\n|--digest|使用数字身份验证|\n| --disable-eprt|禁止使用EPRT或LPRT|\n|--disable-epsv|禁止使用EPSV|\n|-D/--dump-header \\<file>|把header信息写入到该文件中|\n|--egd-file \\<file>|为随机数据(SSL)设置EGD socket路径|\n|--tcp-nodelay|使用TCP_NODELAY选项|\n|-e/--referer|来源网址|\n|-E/--cert \\<cert[:passwd]>|客户端证书文件和密码 (SSL)|\n|--cert-type \\<type>|证书文件类型 (DER/PEM/ENG) (SSL)|\n| --key \\<key>|私钥文件名 (SSL)|\n| --key-type \\<type>|私钥文件类型 (DER/PEM/ENG) (SSL)|\n|--pass \\<pass>|私钥密码 (SSL)|\n| --engine \\<eng>|加密引擎使用 (SSL). \"--engine list\" for list|\n| --cacert \\<file>|CA证书 (SSL)|\n| --capath \\<directory>|CA目录 (made using c_rehash) to verify peer against (SSL)|\n|--ciphers \\<list>|SSL密码|\n|--compressed|要求返回是压缩的形势 (using deflate or gzip)|\n|--connect-timeout \\<seconds>|设置最大请求时间|\n|--create-dirs|建立本地目录的目录层次结构|\n|--crlf|上传是把LF转变成CRLF|\n|-f/--fail|连接失败时不显示http错误|\n|--ftp-create-dirs|如果远程目录不存在，创建远程目录|\n|--ftp-method [multicwd/nocwd/singlecwd]|控制CWD的使用|\n| --ftp-pasv|使用 PASV/EPSV 代替端口|\n|--ftp-skip-pasv-ip|使用PASV的时候,忽略该IP地址|\n|--ftp-ssl|尝试用 SSL/TLS 来进行ftp数据传输|\n|--ftp-ssl-reqd|要求用 SSL/TLS 来进行ftp数据传输|\n|-F/--form \\<name=content>|模拟http表单提交数据|\n|--form-string \\<name=string>|模拟http表单提交数据|\n|-g/--globoff|禁用网址序列和范围使用{}和[]|\n|-G/--get|以get的方式来发送数据|\n|-H/--header \\<line>|自定义头信息传递给服务器|\n|--ignore-content-length|忽略的HTTP头信息的长度|\n|-i/--include|输出时包括protocol头信息|\n|-I/--head|只显示请求头信息|\n|-j/--junk-session-cookies|读取文件进忽略session cookie|\n|--interface \\<interface>|使用指定网络接口/地址|\n|--krb4 \\<level>|使用指定安全级别的krb4|\n|-k/--insecure|允许不使用证书到SSL站点|\n|-K/--config|指定的配置文件读取|\n|-l/--list-only|列出ftp目录下的文件名称|\n|--limit-rate \\<rate>|设置传输速度|\n|--local-port\\<NUM>|强制使用本地端口号|\n|-m/--max-time \\<seconds>|设置最大传输时间|\n| --max-redirs \\<num>|设置最大读取的目录数|\n|--max-filesize \\<bytes>|设置最大下载的文件总量|\n|-M/--manual|显示全手动|\n|-n/--netrc|从netrc文件中读取用户名和密码|\n|--netrc-optional|使用 .netrc 或者 URL来覆盖-n|\n|--ntlm|使用 HTTP NTLM 身份验证|\n|-N/--no-buffer|禁用缓冲输出|\n|-o/--output|把输出写到该文件中|\n|-O/--remote-name|把输出写到该文件中，保留远程文件的文件名|\n|-p/--proxytunnel|使用HTTP代理|\n|--proxy-anyauth|选择任一代理身份验证方法|\n|--proxy-basic|在代理上使用基本身份验证|\n|--proxy-digest|在代理上使用数字身份验证|\n|--proxy-ntlm|在代理上使用ntlm身份验证|\n|-P/--ftp-port \\<address>|使用端口地址，而不是使用PASV|\n|-q|作为第一个参数，关闭 .curlrc|\n|-Q/--quote \\<cmd>|文件传输前，发送命令到服务器|\n|-r/--range \\<range>|检索来自HTTP/1.1或FTP服务器字节范围|\n|--range-file|读取（SSL）的随机文件|\n|-R/--remote-time|在本地生成文件时，保留远程文件时间|\n|--retry \\<num>|传输出现问题时，重试的次数|\n|--retry-delay \\<seconds>|传输出现问题时，设置重试间隔时间|\n|--retry-max-time \\<seconds>\t|传输出现问题时，设置最大重试时间|\n|-s/--silent|静默模式。不输出任何东西|\n|-S/--show-error|显示错误|\n|--socks4 \\<host[:port]>|用socks4代理给定主机和端口|\n|--socks5 \\<host[:port]>|用socks5代理给定主机和端口|\n| --stderr \\<file>||\n|-t/--telnet-option \\<OPT=val>|Telnet选项设置|\n|--trace \\<file>|对指定文件进行debug|\n| --trace-ascii \\<file>|Like --跟踪但没有hex输出|\n|--trace-time|跟踪/详细输出时，添加时间戳|\n|-T/--upload-file \\<file>|上传文件|\n|--url \\<URL>|Spet URL to work with|\n|-u/--user \\<user[:password]>|设置服务器的用户和密码|\n|-U/--proxy-user \\<user[:password]>|设置代理用户名和密码|\n|-w/--write-out [format]|什么输出完成后|\n|-x/--proxy \\<host[:port]>|在给定的端口上使用HTTP代理|\n|-X/--request \\<command>|指定什么命令|\n|-y/--speed-time|放弃限速所要的时间，默认为30|\n|-Y/--speed-limit|停止传输速度的限制，速度时间|\n\n## 实例\n### 文件下载\ncurl命令可以用来执行下载、发送各种HTTP请求，指定HTTP头部等操作。如果系统没有curl可以使用`yum install curl`安装，也可以下载安装。curl是将下载文件输出到stdout，将进度信息输出到stderr，不显示进度信息使用`--silent`选项。\n\n```\ncurl URL --silent\n```\n\n这条命令是将下载文件输出到终端，所有下载的数据都被写入到stdout。\n\n使用选项`-O`将下载的数据写入到文件，必须使用文件的绝对地址：\n\n```\ncurl http://man.linuxde.net/text.iso --silent -O\n```\n\n选项`-o`将下载数据写入到指定名称的文件中，并使用`--progress`显示进度条：\n\n```\ncurl http://man.linuxde.net/test.iso -o filename.iso --progress\n######################################### 100.0%\n```\n\n### 断点续传\n\ncurl能够从特定的文件偏移处继续下载，它可以通过指定一个便宜量来下载部分文件：\n\n```\ncurl URL/File -C 偏移量\n\n#偏移量是以字节为单位的整数，如果让curl自动推断出正确的续传位置使用-C -：\ncurl -C -URL\n```\n\n### 使用curl设置参照页字符串\n参照页是位于HTTP头部中的一个字符串，用来表示用户是从哪个页面到达当前页面的，如果用户点击网页A中的某个连接，那么用户就会跳转到B网页，网页B头部的参照页字符串就包含网页A的URL。\n\n使用`--referer`选项指定参照页字符串：\n\t\t\t\n```\ncurl --referer http://www.google.com http://man.linuxde.net\n```\n\n### 用curl设置cookies\n\n使用`--cookie \"COKKIES\"`选项来指定cookie，多个cookie使用分号分隔：\n\n```\ncurl http://man.linuxde.net --cookie \"user=root;pass=123456\"\n```\n\n将cookie另存为一个文件，使用`--cookie-jar`选项：\n\n```\ncurl URL --cookie-jar cookie_file\n```\n\n### 用curl设置用户代理字符串\n有些网站访问会提示只能使用IE浏览器来访问，这是因为这些网站设置了检查用户代理，可以使用curl把用户代理设置为IE，这样就可以访问了。使用`--user-agent`或者`-A`选项：\n\n```\ncurl URL --user-agent \"Mozilla/5.0\"\ncurl URL -A \"Mozilla/5.0\"\n```\n\n其他HTTP头部信息也可以使用curl来发送，使用`-H`\"头部信息\" 传递多个头部信息，例如：\n\n```\ncurl -H \"Host:man.linuxde.net\" -H \"accept-language:zh-cn\" URL\n```\n\n### curl的带宽控制和下载配额\n使用`--limit-rate`限制curl的下载速度：\n\n```\ncurl URL --limit-rate 50k\n```\n\n命令中用k（千字节）和m（兆字节）指定下载速度限制。\n\n使用`--max-filesize`指定可下载的最大文件大小：\n\n```\ncurl URL --max-filesize bytes\n```\n\n如果文件大小超出限制，命令则返回一个非0退出码，如果命令正常则返回0。\n\n### 用curl进行认证\n\n使用curl选项 -u 可以完成HTTP或者FTP的认证，可以指定密码，也可以不指定密码在后续操作中输入密码：\n\n```\ncurl -u user:pwd http://man.linuxde.net\ncurl -u user http://man.linuxde.net\n```\n\n### 只打印响应头部信息\n\n通过`-I`或者`-head`可以只打印出HTTP头部信息：\n\n```\n[root@localhost text]# curl -I http://man.linuxde.net\nHTTP/1.1 200 OK\nServer: nginx/1.2.5\ndate: Mon, 10 Dec 2012 09:24:34 GMT\nContent-Type: text/html; charset=UTF-8\nConnection: keep-alive\nVary: Accept-Encoding\nX-Pingback: http://man.linuxde.net/xmlrpc.php\n```\n\n[原文地址](http://man.linuxde.net/curl)"
categories:
  - 后端开发
toc: false
date: 2018-01-18 10:10:07
---

curl命令是一个利用URL规则在命令行下工作的文件传输工具。它支持文件的上传和下载，所以是综合传输工具，但按传统，习惯称curl为下载工具。作为一款强力工具，curl支持包括HTTP、HTTPS、ftp等众多协议，还支持POST、cookies、认证、从指定偏移处下载部分文件、用户代理字符串、限速、文件大小、进度条等特征。做网页处理流程和数据检索自动化，curl可以祝一臂之力。

## 语法

```
curl(选项)(参数)
```
<!-- more -->

## 选项
|             选项               |              含义             |
|-------------------------------|-------------------------------|
| -a/--append	                   | 上传文件时，附加到目标文件        |
|-A/--user-agent \<string>	     | 设置用户代理发送给服务器           |
|-anyauth	                      |  可以使用“任何”身份验证方法       |
|-b/--cookie \<name=string/file> |       cookie字符串或文件读取位置  |
|    --basic                    |          使用HTTP基本验         |
|-B/--use-ascii	               |      使用ASCII /文本传输        |
|-c/--cookie-jar \<file>	     |  操作结束后把cookie写入到这个文件中 |
|-C/--continue-at \<offset>|断点续转|
|-d/--data \<data>|HTTP POST方式传送数据|
| --data-ascii \<data>|以ascii的方式post数据|
|--data-binary \<data>|以二进制的方式post数据|
| --negotiate|使用HTTP身份验证|
|--digest|使用数字身份验证|
| --disable-eprt|禁止使用EPRT或LPRT|
|--disable-epsv|禁止使用EPSV|
|-D/--dump-header \<file>|把header信息写入到该文件中|
|--egd-file \<file>|为随机数据(SSL)设置EGD socket路径|
|--tcp-nodelay|使用TCP_NODELAY选项|
|-e/--referer|来源网址|
|-E/--cert \<cert[:passwd]>|客户端证书文件和密码 (SSL)|
|--cert-type \<type>|证书文件类型 (DER/PEM/ENG) (SSL)|
| --key \<key>|私钥文件名 (SSL)|
| --key-type \<type>|私钥文件类型 (DER/PEM/ENG) (SSL)|
|--pass \<pass>|私钥密码 (SSL)|
| --engine \<eng>|加密引擎使用 (SSL). "--engine list" for list|
| --cacert \<file>|CA证书 (SSL)|
| --capath \<directory>|CA目录 (made using c_rehash) to verify peer against (SSL)|
|--ciphers \<list>|SSL密码|
|--compressed|要求返回是压缩的形势 (using deflate or gzip)|
|--connect-timeout \<seconds>|设置最大请求时间|
|--create-dirs|建立本地目录的目录层次结构|
|--crlf|上传是把LF转变成CRLF|
|-f/--fail|连接失败时不显示http错误|
|--ftp-create-dirs|如果远程目录不存在，创建远程目录|
|--ftp-method [multicwd/nocwd/singlecwd]|控制CWD的使用|
| --ftp-pasv|使用 PASV/EPSV 代替端口|
|--ftp-skip-pasv-ip|使用PASV的时候,忽略该IP地址|
|--ftp-ssl|尝试用 SSL/TLS 来进行ftp数据传输|
|--ftp-ssl-reqd|要求用 SSL/TLS 来进行ftp数据传输|
|-F/--form \<name=content>|模拟http表单提交数据|
|--form-string \<name=string>|模拟http表单提交数据|
|-g/--globoff|禁用网址序列和范围使用{}和[]|
|-G/--get|以get的方式来发送数据|
|-H/--header \<line>|自定义头信息传递给服务器|
|--ignore-content-length|忽略的HTTP头信息的长度|
|-i/--include|输出时包括protocol头信息|
|-I/--head|只显示请求头信息|
|-j/--junk-session-cookies|读取文件进忽略session cookie|
|--interface \<interface>|使用指定网络接口/地址|
|--krb4 \<level>|使用指定安全级别的krb4|
|-k/--insecure|允许不使用证书到SSL站点|
|-K/--config|指定的配置文件读取|
|-l/--list-only|列出ftp目录下的文件名称|
|--limit-rate \<rate>|设置传输速度|
|--local-port\<NUM>|强制使用本地端口号|
|-m/--max-time \<seconds>|设置最大传输时间|
| --max-redirs \<num>|设置最大读取的目录数|
|--max-filesize \<bytes>|设置最大下载的文件总量|
|-M/--manual|显示全手动|
|-n/--netrc|从netrc文件中读取用户名和密码|
|--netrc-optional|使用 .netrc 或者 URL来覆盖-n|
|--ntlm|使用 HTTP NTLM 身份验证|
|-N/--no-buffer|禁用缓冲输出|
|-o/--output|把输出写到该文件中|
|-O/--remote-name|把输出写到该文件中，保留远程文件的文件名|
|-p/--proxytunnel|使用HTTP代理|
|--proxy-anyauth|选择任一代理身份验证方法|
|--proxy-basic|在代理上使用基本身份验证|
|--proxy-digest|在代理上使用数字身份验证|
|--proxy-ntlm|在代理上使用ntlm身份验证|
|-P/--ftp-port \<address>|使用端口地址，而不是使用PASV|
|-q|作为第一个参数，关闭 .curlrc|
|-Q/--quote \<cmd>|文件传输前，发送命令到服务器|
|-r/--range \<range>|检索来自HTTP/1.1或FTP服务器字节范围|
|--range-file|读取（SSL）的随机文件|
|-R/--remote-time|在本地生成文件时，保留远程文件时间|
|--retry \<num>|传输出现问题时，重试的次数|
|--retry-delay \<seconds>|传输出现问题时，设置重试间隔时间|
|--retry-max-time \<seconds>	|传输出现问题时，设置最大重试时间|
|-s/--silent|静默模式。不输出任何东西|
|-S/--show-error|显示错误|
|--socks4 \<host[:port]>|用socks4代理给定主机和端口|
|--socks5 \<host[:port]>|用socks5代理给定主机和端口|
| --stderr \<file>||
|-t/--telnet-option \<OPT=val>|Telnet选项设置|
|--trace \<file>|对指定文件进行debug|
| --trace-ascii \<file>|Like --跟踪但没有hex输出|
|--trace-time|跟踪/详细输出时，添加时间戳|
|-T/--upload-file \<file>|上传文件|
|--url \<URL>|Spet URL to work with|
|-u/--user \<user[:password]>|设置服务器的用户和密码|
|-U/--proxy-user \<user[:password]>|设置代理用户名和密码|
|-w/--write-out [format]|什么输出完成后|
|-x/--proxy \<host[:port]>|在给定的端口上使用HTTP代理|
|-X/--request \<command>|指定什么命令|
|-y/--speed-time|放弃限速所要的时间，默认为30|
|-Y/--speed-limit|停止传输速度的限制，速度时间|

## 实例
### 文件下载
curl命令可以用来执行下载、发送各种HTTP请求，指定HTTP头部等操作。如果系统没有curl可以使用`yum install curl`安装，也可以下载安装。curl是将下载文件输出到stdout，将进度信息输出到stderr，不显示进度信息使用`--silent`选项。

```
curl URL --silent
```

这条命令是将下载文件输出到终端，所有下载的数据都被写入到stdout。

使用选项`-O`将下载的数据写入到文件，必须使用文件的绝对地址：

```
curl http://man.linuxde.net/text.iso --silent -O
```

选项`-o`将下载数据写入到指定名称的文件中，并使用`--progress`显示进度条：

```
curl http://man.linuxde.net/test.iso -o filename.iso --progress
######################################### 100.0%
```

### 断点续传

curl能够从特定的文件偏移处继续下载，它可以通过指定一个便宜量来下载部分文件：

```
curl URL/File -C 偏移量

#偏移量是以字节为单位的整数，如果让curl自动推断出正确的续传位置使用-C -：
curl -C -URL
```

### 使用curl设置参照页字符串
参照页是位于HTTP头部中的一个字符串，用来表示用户是从哪个页面到达当前页面的，如果用户点击网页A中的某个连接，那么用户就会跳转到B网页，网页B头部的参照页字符串就包含网页A的URL。

使用`--referer`选项指定参照页字符串：
			
```
curl --referer http://www.google.com http://man.linuxde.net
```

### 用curl设置cookies

使用`--cookie "COKKIES"`选项来指定cookie，多个cookie使用分号分隔：

```
curl http://man.linuxde.net --cookie "user=root;pass=123456"
```

将cookie另存为一个文件，使用`--cookie-jar`选项：

```
curl URL --cookie-jar cookie_file
```

### 用curl设置用户代理字符串
有些网站访问会提示只能使用IE浏览器来访问，这是因为这些网站设置了检查用户代理，可以使用curl把用户代理设置为IE，这样就可以访问了。使用`--user-agent`或者`-A`选项：

```
curl URL --user-agent "Mozilla/5.0"
curl URL -A "Mozilla/5.0"
```

其他HTTP头部信息也可以使用curl来发送，使用`-H`"头部信息" 传递多个头部信息，例如：

```
curl -H "Host:man.linuxde.net" -H "accept-language:zh-cn" URL
```

### curl的带宽控制和下载配额
使用`--limit-rate`限制curl的下载速度：

```
curl URL --limit-rate 50k
```

命令中用k（千字节）和m（兆字节）指定下载速度限制。

使用`--max-filesize`指定可下载的最大文件大小：

```
curl URL --max-filesize bytes
```

如果文件大小超出限制，命令则返回一个非0退出码，如果命令正常则返回0。

### 用curl进行认证

使用curl选项 -u 可以完成HTTP或者FTP的认证，可以指定密码，也可以不指定密码在后续操作中输入密码：

```
curl -u user:pwd http://man.linuxde.net
curl -u user http://man.linuxde.net
```

### 只打印响应头部信息

通过`-I`或者`-head`可以只打印出HTTP头部信息：

```
[root@localhost text]# curl -I http://man.linuxde.net
HTTP/1.1 200 OK
Server: nginx/1.2.5
date: Mon, 10 Dec 2012 09:24:34 GMT
Content-Type: text/html; charset=UTF-8
Connection: keep-alive
Vary: Accept-Encoding
X-Pingback: http://man.linuxde.net/xmlrpc.php
```

[原文地址](http://man.linuxde.net/curl)