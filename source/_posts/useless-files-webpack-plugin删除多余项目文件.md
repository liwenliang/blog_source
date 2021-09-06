---
title: useless-files-webpack-plugin删除多余项目文件
tags:
  - 知识点备忘
  - Web开发
originContent: ''
categories:
  - Web开发
toc: false
date: 2021-07-01 11:55:09
---

随着项目开发的进行会经历一个混乱到整齐划一的过程，这个过程中会产生一系列冗余代码，这时候通过useless-files-webpack-plugin这个插件，可以帮助我们删除项目中无用的文件；


> 大多数项目随着时间的推移,经常会摒弃掉一部分功能, 但大多数只是入口关闭, 或注释掉功能代码块, 依赖的文件如果没删除掉,项目其他成员一般不会去处>理. 在日常开发中, 通过webpack 生成的 stats 文件实现了去除多余文件的插件useless-files-webpack-plugin

> 插件用法如下:

```javascript
const UselessFile = require('useless-files-webpack-plugin')
 
plugins: [
  new UselessFile({
    root: './src', // 项目目录
    out?: './fileList.json', // 输出文件列表
    out?: (files) => deal(files), // 或者回调处理
    clean?: false // 删除文件,
    exclude?: path // 排除文件列表, 格式为文件路径数组
  })
]
```

> 转载于:https://juejin.im/post/5ae8930751882567244dc72f