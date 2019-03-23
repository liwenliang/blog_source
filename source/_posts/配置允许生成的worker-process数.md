---
title: 配置允许生成的worker process数
tags:
  - nginx
originContent: |-
  ```
  worker_processed number | auto
  ```
  > number, 指定Nginx进程最多可以产生的work process数
  > auto，设置此值，Nginx进程将自动检测
categories:
  - 后端开发
toc: false
date: 2017-08-27 07:27:58
---

```
worker_processed number | auto
```
> number, 指定Nginx进程最多可以产生的work process数
> auto，设置此值，Nginx进程将自动检测