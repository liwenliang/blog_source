---
title: 设置一个npm的别名
tags:
  - Web开发
originContent: ''
categories:
  - NodeJS
  - Web开发
toc: false
date: 2021-12-27 17:20:46
---

设置jnpm

```bash
alias jnpm="npm --registry=http://registry.m.jd.com \
--cache=$HOME/.npm/.cache/jnpm \
--disturl=https://npmmirror.com/dist \
--userconfig=$HOME/.jnpmrc"
```

设置cnpm
```bash
alias cnpm="npm --registry=https://registry.npmmirror.com \
--cache=$HOME/.npm/.cache/cnpm \
--disturl=https://npmmirror.com/dist \
--userconfig=$HOME/.cnpmrc"
```

