---
title: Rewrite功能的配置（1）
date: 2017-09-02 17:21:17
tags: nginx
---
![游戏机](http://houjiyi.oss-cn-beijing.aliyuncs.com/images/80%E5%90%8E/1cd4edf0c7c6d0e7a7c44120d7197168.jpg)

Rewrite 是 Nginx 服务器提供的一个重要基本功能，骑在Web服务器产品中几乎是必备的功能，用于实现URL的重写。URL的重写是非常有用的功能，比如他可以让我们在改变网站结构后，无需要求客户端用户修改原来的书签，也无需其他网站修改对我们网站的友情链接；他还可以在一定程度上提高网站的安全性；能够让我们的网站显得更加专业。

<!-- more -->

适当利用Rewrite功能，可以给我们带来很多好处，这一节我们就重点学习一下 Nginx 服务器的Rewrite功能。Nginx服务器的Rewrite功能的实现依赖于PCRE（Perl Compatible Regular Expressions，Perl兼容的正则表达式）的支持，因此在编译安装Nginx服务器之前，需要安装PCRE库。

> 提示：有关PCRE的介绍和PCRE库的相关下载请参阅其[官方网站](http://www.pcre.org/)。
> Nginx服务器使用ngx_http_rewrite_module模块解析和处理Rewrite功能的相关配置。

## 『地址重写』与『地址转发』

『地址重写』与『转发』在计算机网络领域是两个重要概念，经常被大家提起。但许多人对这两个概念的区别不清楚，甚至混为一谈。在学习Nginx服务器的Rewrite功能之前，我们有必要先将这对概念进行一下分析和对比，让大家能够有一个清晰的认识。

『地址重写』，实际上是为了实现地址标准化。那么，什么是地址标准化呢？我们来举一个例子。比如在访问Google首页的时候，我们在地址栏中可以输入www.google.com，也可以输入google.cn，他们都能够准确的指向Google首页，从客户端来看，Google首页同时对应了两个地址，实际上，Google服务器是在不同的地址中选择了确定的一个，即www.google.com，进而返回服务器响应的。这个过程就是地址标准化的过程。google.cn这个地址在服务器中被改变为www.google.com的过程就是地址重定向的过程。

『转发』的概念最初和网页的访问并没有太大关系，他是指在网络数据传输过程中数据分组到达路由器或者桥接器后该设备通过检查分组地址并将数据转到相邻局域网上的过程。后来该概念被用在网页访问中，出现了『地址转发』的说法。『地址转发』是指将一个域名知道另一个已有站点的过程。

从上面的解释，我们可以看到『地址重写』和『地址转发』代表的两个过程是不同的。我们可以总结这两个过程的几点区别：

> 地址转发后客户端浏览器中的地址显示时不改变的；而地址重写后客户端浏览器地址栏中的地址改变为服务器选择确定的地址。
> 在一次地址转发整个过程中，只产生一次网络请求；而一次地址重写一般会产生两次请求。
> 地址转发一般发生在统一站点项目内；二地址重写没有该限制
> 地址转发到的页面可以不用全路径名标识，二地址重写到的页面必须使用完整的路径名表示
> 地址转发过程中，可以将客户端请求的request范围内属性传递给新的页面，但地址重写不可以
> 地址转发的速度较地址重定向快。

## Rewrite规则
Rewrite规则是学习和使用Nginx服务器Rewrite功能的基础，可以借助PCRE实现URI的重写，并且她还支持Nginx预设变量。Rewrite规则的核心就是PCRE。

正则表达式（Regular Expression，缩写为regex、regexp或RE），是用于描述或者匹配一系列符合某个句法规则的字符串的一个字符串。大家应该都是用过Windows/Dos平台下用于文件查找的通配符"\*" 和 "?"，如果要查找某个目录下所有的word文档，就是用"\*.doc"进行搜索，『\*』会被解释成替换符合某个句法模式的文本内容。许多程序设计语言都支持利用正则表达式进行字符串操作。PCRE就是在Perl中内建的功能强大的正则表达式引擎，可以被许多工具使用的库。正则表达式有多种不同的分割，并且不同版本再对句法规则的匹配处理上也有一定的差异。

在Nginx服务器中，使用ngx_http_rewrite_module模块支持URL重写功能。该模块是Nginx服务器的标准HTTP模块。

由于正则表达式的知识超出了本书的范围，我们不在正文中对其详细阐述。在『附录A』中，笔者为大家整理了PCRE库支持的正则表达式元字符表，并对这些元字符在正则表达式上下文中的行为进行了说明，同时也列举了大量的使用实例供大家参考。

在这里，建议大家学习正则表达式时，一定要将思维从『字符创』的概念转变为『字符』的概念，将『词句』的概念转变为『字』的概念，多结合实例进行操作，这样才能准确理解和运用正则表达式。另外，网络上有不少PCRE正则表达式的测试工具，大家可以搜索下载，这些工具对我们在学习和使用正则表达式的过程中很有帮助。

在Nginx配置中，有关Rewrite的配置指令不多，但是它们已经能够提供比较完善的功能了。

### if指令
该指令用来支持条件判断，并根据条件判断结果选择不同的Nginx配置，可以在server块或location块中配置该指令，其语法结构为：
```
if ( condition ) {...}
```

其中，花括号代表一个作用域，形成一个if配置块，是条件为真时的Nginx配置。condition为判断条件（true、false），他可以支持以下几种设置方法：
#### 变量名。
如果变量的值为空字符串或以『0』开头的任意字符串，if指令认为条件为false，其他情况认为条件为true。比如：

```
if ($slow) {
  ... #Nginx 配置
}
```

#### 使用'='（等于）和'!='（不等于）
比较变量和字符串是否相等，相等时if指令为true，反之为false。

```
if ($request_method = POST) {
	return 405;
}
```
> 注意：这里的字符串不需要加引号。

#### 使用正则表达式对变量进行匹配
匹配成功时 if 指令认为条件为true，否则为false。变量与正则表达式之间用"~"、"~*"、"!~"或"!~\*"连接，『~』表示匹配过程中对大小写敏感，『~\*』表示匹配过程中对大小写不敏感；使用『!~』和"!~\*"，匹配失败时if指令认为条件为true，否则为false。在正则表达式中，可以使用小括号对变量值进行截取，在花括号中使用$1...$9引用截取的值。比如：

```
if ($http_user_agent ~ MSIE) {
	# $http_user_agent的值中是否包含有MSIE字符串，如果包含，为true
	...
}
```



> 注意：整个正则表达式字符串一般不需要加引号，但如果含有右花括号"}"或者分号";"字符时，必须要给整个正则表达式添加引号。

#### 判断请求的文件是否存在使用"-f"和"!-f"。
当使用"-f"时，如果请求的文件存在，if指令认为条件为true，如果请求的文件不存在为false；使用"!-f"时，如果请求的文件不存在但该文件所在的目录存在，if指令认为条件为true，如果该文件和她所在的目录都不存在，则为false，如果请求的文件存在，也为false。使用的方法如下：

```
if (-f $request_filename) {
	# 判断请求的文件是否存在
	...
}
if (!-f $request_filename) {
	# 判断请求的文件是否不存在
}
```

#### 判断请求的目录是否存在使用"-d"和"!-d".
当使用"-d"时，如果请求的目录存在，if指令认为条件为true，如果请求的目录不存在，则为false；当使用"!-d"时，如果请求的目录不存在但该目录的上级目录存在，if指令认为条件为true，如果该目录和它的上级目录都不存在，则为false，如果请求的目录存在，也为false。使用方式见"-f"和"!-f"的使用。

#### 判断请求的目录或文件是否存在使用"-e"和"!-e"。
当使用"-e"时，如果请求的目录或者文件存在时，if指令认为条件为true，否则为false。当使用"!-e"时，如果请求的文件和该文件所在路径上的目录都不存在，为true，否则为false。使用方式见"-f"和"!-f"的使用。
#### 判断请求的文件是否可执行使用"-x"和"!-x"。
当使用"-x"时，如果请求的文件可执行，if指令认为条件为true，否则为false；当使用"!-x"时，如果请求的文件不可执行，为true，否则为false。使用方法见"-f"和"!-f"的使用

### break指令
该指令用于中断当前相同作用域中的其他Nginx配置。与该指令处于同一作用域的Nginx配置中，位于它前面的指令配置生效，位于后面的指令配置无效。Nginx服务器在根据配置处理请求的过程中遇到该指令时，回到上一层作用域继续向下读取配置。该指令可以在server块和location快以及if块中使用，其语法结构为：

```
break;
```

我们通过一个例子加深理解：

```
location / {
	if ($slow) {
		set $id $1;			#处于break指令之前，配置有效
		break;
		limit_rate 10k;		#处于break指令之后，配置无效
	}
	...							#其他Nginx配置，处于break指令所在作用域的上一层作用域，配置有效
}
```

#### return指令
该指令用于完成对请求的处理，直接向客户端返回响应状态码。处于该指令后的所有Nginx配置都是无效的。该指令可以在server块和location块以及if块中使用，其语法结构有以下几种：

```
return [text];
return code URL;
return URL;
```

> code，为返回给客户端的HTTP状态代码。可以返回的状态代码为0-999的任意HTTP状态码。非标准的444代码可以强制关闭服务器与客户端的连接而不返回任何响应信息给客户端。
> 
> text，为返回给客户端的响应体内容，支持变量的使用。`return 200 'hello from nginx;'`
> 
> URL，为返回给客户端的URL地址。

从Nginx 0.8.42 开始，当code使用301（表示被请求资源永久移动到新的位置）、302（表示请求的资源现在临时从不同的URL响应，要求使用GET昂视请求）、303（表示对应当前请求的相应可以在另一个URL上找到，并且客户端应当采用GET方式访问那个资源）和307（请求的资源临时从不同的URL响应）代码时，可以使用结构2将新的URL返回给客户端；当code使用除上面提到的其他代码时，可以使用结构1指定text想客户端发送指定的响应体内容。

当返回状态吗未302或307时，可以使用结构3对URL进行配置。返回的URL中应该包含"http://"、"https://"或者直接使用"$scheme"变量（Request Scheme，代表传输协议，Nginx内置变量）指定。

> 注意：在Nginx 0.7.51之前的版本中，只支持返回204、400、402-406、608、410、413、416和500-504等状态代码。

### rewrite指令
该指令通过正则表达式的使用来改变URI。可以同时存在一个或者多个指令，按照顺序依次对URL进行匹配和处理。
> 提示：
> 
> URI与URL的区别和联系。
> 
> URI（Universal Resource Identifier，通用资源标识符），用于对网络中的各种资源进行标识，由存放资源的主机名、片段标识符和相对URI三部分组成。存放资源的主机名一版由传输协议（Scheme）、主机和资源路径三部分组成；片段标识符指向资源内容的具体元素；相对URI表示资源在主机上的相对路径。一版格式为：Scheme:[//][用户名[:密码]@主机名[:端口号]][/资源路径]。
> 
> URL（Uniform Resource Location，统一资源定位符），是用于在Interner中描述资源的字符串，是URI的子集，主要包括传输协议（Scheme）、主机（IP、端口号或者域名）和资源具体地址（目录和文件名）等三部分。一版格式为：Scheme://主机名[:端口号][/资源路径]。

该指令可以在server块或者location块中配置，其语法结构为：
```
rewrite regex replacement [flag];
```

> regex，用于匹配URI的正则表达式。使用"()"标记要截取的内容。

注意：

rewrite接收到的URI不包括host地址。因此，regex不可能匹配到URI的host地址。我们看下面这个例子：

```
rewrite myweb.com http://newweb.com/parmanent;
```

现在我们希望上面的rewrite指令重写http://myweb.com/source是办不到的，因为rewrite指令接收到的URI是"/source"，不包含"myweb.com"。

另外，请求URL中的请求指令是不包含在rewrite指令接收到的URI内容中的。比如:

`http://myweb.com/source?agr1=value&agr2=value2`;

rewrite指令接收到奥的URI为"source",不包含"?agr1=value&agr2=value2"。

> replacement，成功匹配后用于替换URI中被截取内容的字符串。默认情况下，如果该字符串是由"http://"或者"https://"开头的，则不会继续向下对URI进行其他处理，而是直接将重写后的URI返回给客户端。

提示：

刚才学习regex变量时我们提到，rewrite模块快接收到的URI不包含请求URL中的请求指令，但是如果我们希望将这些指令传给重写后的URI，该怎么做呢？我们可以使用Nginx全局变量$request_uri，比如：
`rewrite myweb.com http://example.com$request_uri? permanent;`

注意：

在$request_uri变量后要添加问号"? ".replacement变量中支持Nginx全局变量的使用，常用的还有$uri和$args等。

> flag，用于设置rewrite对URI的处理行为，可以为以下标志中的一个：
>> last，终止继续在本location块中处理接收到的URI，并将此处重写的URI作为一个新的URI，使用各location块进行处理。该标识将重写后徐的URI重新再server块中执行，为重写后的URI提供了转入到其他location块的机会。我们通过一个例子来加深理解：
>> 
```
location / {
	rewrite ^(/myweb/.*)/media/(.*)\..*$ $1/mp3/$2.mp3 last;
	rewrite ^(/myweb/.*)/audio/(.*)\.*$ $1/mp3/$2.ra last;
}
```
>> 如果URI在第二行被匹配成功并处理，Nginx服务器不会继续使用第三行的配置匹配和处理新的URI，而是让所有的location块重新匹配和处理新的URI。
>> 
>> break，将此处重写的URI作为一个新的URI，在本快中继续进行处理。该标志将重写后的地址在当前的location块中执行，不会讲新的URI专项到其他的location块。看下面的例子：
>> 
```
location /myweb/ {
	rewrite ^(/myweb/.*)/media/(.*)..*$ $1/mp3/$2.mp3 break;
	rewrite ^(/myweb/.*)/audio/(.*)\.*$ $1/mp3/$2.ra break;
}
```
如果某URI在第二行被匹配成功并处理，Nginx服务器将新的URI继续在该location块中使用第三行进行匹配和处理。新的URI始终是在同一个location块中。
>> 
>> redirect, 将重写后的URI返回给客户端，状态代码为302，指明是临时重定向的URI，主要用在replacement变量不是以"http://"或者"https://"开头的情况下。
>> 
>> permanent，将重写后的URI返回给客户端，状态码为301，指明是永久重定向URI。

在使用flag指令时，一定要注意各个标志之间的配合。我们再来回顾刚才学习break标志时的例子，对比last标志里的例子，如果我们将第二个例子中的break标志换成last标志，会发生什么情况呢？

细心的读者可能已经发现，在第二个例子中，location块的uri指令是"/myweb/"，而重写后的URI仍然是包含"/myweb/"的，如果使用last标志，重写后的URI还可能会被该location块匹配到，这样就形成了无限循环。Nginx服务器遇到这样的情况，会尝试10次循环之后返回错误状态代码500。


### rewrite_log指令
该指令配置是否开启URL重写日志的输出功能，其语法结构为：
```
rewrite_log on | off;
```
默认设置为off。如果配置为开启（on),URL重写的相关日志将以notice级别输出到error_log指令配置的日志文件中。

### set指令
该指令用于设置一个新的变量，其语法结构为：

```
set variable value;
```
> variable，为变量的名称。注意要用符号"$"作为变量的第一个字符，且变量不能与Nginx服务器预设的全局变量同名。
> 
> value，为变量的值，可以使字符串、其他变量或者变量的组合等。

### uninitialized\_variable\_warn指令
该指令用于配置使用为初始化的变量时，是否记录警告日志，其语法结构为：

```
uninitialized_variable_warn on | off;
```
默认设置为开启（on）状态。

### Rewrite 常用全局变量
|变量|说明|
|:---|:---|
|**$args**|变量中存放了请求URL中的请求指令。比如http://www.myweb.name/server/source?arg1=value1&arg2=value2中的"arg1=value1&arg2=value2"|
|$content_length|变量中存放了请求头中的Content-length字段|
|$content_type|变量中存放了请求头中的Content-type字段|
|$document_root|变量中存放了针对当前请求的根路径|
|$document_uri|变量中存放了请求中的当前URI，并且不包括请求指令，比如http://www.myweb.name/server/source?arg1=value1&arg2=value2中的"/server/source"|
|**$host**|变量中存放了请求URL中的主机部分字段，比如http://www.myweb.name/server中的"www.myweb.name"。如果请求中的主机部分字段不可用或者为空，则存放Nginx配置中server块中server_name指令的配置值|
|$http_user_agent|变量中存放客户端的代理信息|
|**$http_cookie**|变量中存放客户端的cookie信息|
|$limit_rate|变量中存放Nginx服务器对网络连接速率的限制，也就是Nginx配置中limit_rate指令的配置值|
|**$remote_addr**|变量中存放了客户端的地址|
|$remote_port|变量中存放了客户端与服务器建立连接的端口号|
|$remote_user|变量中存放了客户端的用户名|
|$request_body_file|变量中存放了发给后端服务器的本地文件资源的名称|
|**$request_method**|变量中存放了客户端的请求方式，如『GET』、『POST』等|
|$request_filename|变量中存放了当前请求的资源文件的路径名|
|**$request_uri**|变量中存放了当前请求的URI，并且带请求指令|
|**$query_string**|与变量$args含义相同|
|**$scheme**|变量中存放了客户端请求使用的协议，比如"http"、"https"和"ftp"等|
|$server_protocol|变量中存放了客户端请求协议的版本，比如"HTTP/1.0"、"HTTP/1.1"等|
|$server_addr|变量中存放了服务器的地址|
|$server_name|变量中存放了客户端请求到达的服务器的名称|
|$server_port|变量中存放了客户端请求到达的服务器的端口号|
|$uri|与变量$document_uri含义相同|