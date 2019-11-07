---
title: 更改location的URI
tags:
  - nginx
categories:
  - 后端开发
toc: false
date: 2017-08-27 18:55:04
---

在location块中，除了使用root指令指明请求处理根目录，还可以使用alias指令改变location接收到的URI的请求路径，其语法结构为：

<!-- more -->

```
alias path;
```

其中，path即为修改后的根路径。同样，次变量中也可以包含除了$document_root和$realpath_root之外的其他Nginx服务器预设变量。

这个指令的作用有点不好理解，我们来看一个示例：

```
location ~ ^/data/(.+\.(html|htm))$ {
    alias   /locationtest1/other/$1;
}
```

当此location块接收到'/data/index.html'的请求时，匹配成功，之后根据alias指令的配置，Nginx服务器将到/locationtest1/other目录下找到index.html并相应请求。可以看到，通过alias指令的配置，根路径已经从/data更改为/locationtest1/other了。

> 如果没有alias设置，那么/data/index.html对应的文件路径就是[root_path]/data/index.html，这个设置这里我觉得最大的用处应该是为了减少路由的层级，比如我们为一个相对复杂的路径设置一个别名，这个别名对应一个简单的路径，在开发和使用的时候会相对简单一些，同时可以解耦一些业务，这个简易路径可以针对具体后端业务场景，而前端场景则可以相对随意一些，设置一个别名指向这个简易路径即可。
