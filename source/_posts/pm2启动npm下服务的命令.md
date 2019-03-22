---
title: pm2启动npm下服务的命令
date: 2018-03-27 14:40:07
tags: [pm2, npm]
---
有时候我们的项目只有npm命令执行启动，这时候还想用pm2来管理该怎么做呢：
<!-- more -->

```
# 其中run start就是npm指令 npm run start
pm2 start npm --name 'yourprojectname' -- run start
```