---
title: 长时间Running flutter pub get没反应
tags:
  - 知识点备忘
categories:
  - 移动开发
toc: false
date: 2020-08-10 17:47:58
---

先说结论就是环境变量配置的又问题：

肯定是这个配置：
```bash
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```

为什么还是不行的原因就是我安装了zsh，.bash_profile配置了不好使，于是需要在.zshrc里也配置上才行。