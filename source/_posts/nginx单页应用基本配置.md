---
title: nginx单页应用基本配置
date: 2018-03-25 10:10:09
tags: [nginx, 单页应用]
---

我们经常会用到nginx单页应用，同时我们部署的时候需要注意一点就是单页应用的所有页面入口都是相同的, nginx上怎么配置呢

<!-- more -->
如下：

```
server {
    listen 80;
    charset utf-8;
    root /your/root;
    index index.html index.htm;
    location / {
        try_files $uri $uri/ /index.html;
    }
}
```
