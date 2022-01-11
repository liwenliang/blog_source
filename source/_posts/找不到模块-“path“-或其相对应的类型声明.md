---
title: 找不到模块 “path“ 或其相对应的类型声明
tags:
  - 知识点备忘
originContent: ''
categories:
  - Web开发
toc: false
date: 2022-01-05 15:57:28
---

使用 npm init @vitejs/app 创建项目添加别名

```javascript
import path from “path”;
```
有这个错误提示：

```language
找不到模块 ‘path’ 或其相对应的类型声明
找不到名称"__dirname"
```

解决方法：

```bash
npm install @types/node --save-dev
```

```javascript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import path from 'path'

// 1. defineConfig: 不用 jsdoc 注解也可以获取类型提示
// 2. 找不到模块“path”或其相应的类型声明 或者 找不到名称“__dirname 安装 @types/node

const resolve = (dir: string) => path.join(__dirname, dir)

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      '@': resolve('src'),
      'comps': resolve('src/components'),
      'apis': resolve('src/apis'),
      'views': resolve('src/views'),
      'utils': resolve('src/utils'),
      'routes': resolve('src/routes'),
      'styles': resolve('src/styles')
    }
  },
})


```


