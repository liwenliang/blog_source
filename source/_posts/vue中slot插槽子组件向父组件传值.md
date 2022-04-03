---
title: vue中slot插槽子组件向父组件传值
tags:
  - vue
originContent: ''
categories:
  - Web开发
toc: false
date: 2022-04-03 16:54:03
---

slot 插槽要实现子组件向父组件传值，则需要运用 **作用域插槽**

1、父组件中用 标签加上 slot-scoped 的属性，属性值随性。（旧版本是scope,vue新版本必须用slot-scope)

2、子组件中给 加上一个自定义属性，在父组件中的slot-scope就能接收这个自定义属性的值

`父组件`
```html
<template slot-scope="props" slot="append-operate">
    <p>{{ props.msg }}</p>
</template>
```

`子组件`
```html
<slot name="append-operate" msg="我是子组件的内容"></slot>
```

