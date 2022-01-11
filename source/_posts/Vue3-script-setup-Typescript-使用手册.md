---
title: Vue3 + <script setup> + Typescript 使用手册
tags:
  - 知识点备忘
originContent: ''
categories:
  - Web开发
toc: false
date: 2022-01-06 20:04:16
---

![image.png](https://blogimage.houjiyi.com/FhSlsNqzkvmtYg06KBFkblIfvwQ3)

## 前言
相信看到这面文章的你对新技术一定充满好奇，并且已经对 vue3 提供的 script setup 的写法爱不释手，但目前该写法尚在完善中，所以还是有许多坑需要踩的，文档暂时也不全，本文主要列举一下 Vue3 + Vite + script setup 在 Typescript 中的使用，希望能帮你愉快的使用好它。

## name 组件名字
给组件命名可以方便在 devtool 调试工具中更快定位组件

### 方法1：多写一个 script 标签
```typescript
<script lang="ts">
export default { name: 'MyComponent' };
</script>
<script setup lang="ts">
// 组件内容
</script>
```

### 方法2：vue 文件命名
如：MyComponent.vue

## data( ref / reactive / computed )
```typescript
<script setup lang="ts">
import { computed, reactive, ref } from '@vue/runtime-core';
type User = { name: string; password: string };

// ref
const msg = ref(''); //  会默认约束成 string 类型
const msg2 = ref<string>(''); //  可以通过范型约束类型
const user = ref({} as User); // 自定义类型强制转化 通过 user.value. 访问

// reactive
const obj = reactive({});
const user2 = reactive<User>({name: '', password: ''});
const user3 = reactive({} as User); // 自定义类型强制转化

// computed
const str3 = computed(() => ''); // 会默认约束成 string 类型
const user5 = computed<User>(() => {
  return { name: '', password: '' };
});
</script>
```


## props

使用 defineProps 以及 as PropType 定义 typescript类型

```typescript
<script setup lang="ts">
import { defineProps } from '@vue/runtime-core';
// 特别注意：接口类型需要使用 import type 否则编译出错
import type { PropType } from '@vue/runtime-core'; 
type User = { name: string; password: string };

const props = defineProps({
  str: {
    type: String as PropType<string>,
    // string 首字母小写为 typescript 对应 js 中的基础类型
    default: ''
  },
  user: {
    type: Object as PropType<User>,
    default() {
      return {};
    },
  },
});
</script>
```

## Template Refs

```html
<template>
    <div ref="divRef"></div>
    <div :ref="(el) => setItemRef"></div>
</template>
```

```typescript
<script setup lang="ts">
import { ref, useContext } from '@vue/runtime-core';

// const divRef = ref<HTMLElement>({} as HTMLElement); // 新版本已支持如下写法
const divRef = ref({} as HTMLElement);
const itemRefs = ref([] as Array<HTMLElement>);
function setItemRef(el: HTMLElement) {
  if (el) {
    itemRefs.value.push(el);
  }
}
</script>
```

## Component 组件化
使用 Typescript 可以更好的提示组件方法。

```typescript
<!-- components/Dialog.vue -->
<template>...</template>
<script setup lang="ts">
import { useContext } from '@vue/runtime-core';

function open() {
  console.log('open dialog');
}
function close() {
  console.log('close dialog');
}
// 需要手动 expose 需要暴露给外部的方法
useContext().expose({
  open,
  close,
});
</script>
```

```typescript
// src/components/dialog.d.ts 添加额外接口信息
export interface IDialog {
  open: Function;
  close: Function;
}
```

```typescript
<template>
  <Dialog ref="dialogRef"></Dialog>
</template>

<script setup lang="ts">
// @ts-ignore
import Dialog from '@/components/Dialog.vue'; // 引入组件
import type { IDialog } from '@/components/dialog'; // 引入类型信息
import { ref } from '@vue/reactivity';

const dialogRef = ref({} as IDialog); // 将组件绑定接口信息
dialogRef.value.open(); // 这样在输入dialogRef.value. 时就会提示出已暴露的方法了
</script>
```
![image.png](https://blogimage.houjiyi.com/FjrtqnrfwOHVM9HxuRcbc6PWK1K7)

## 总结

虽然在使用 script setup 的时候经常无迹可寻，但这种写法真的非常简洁，未来可期。希望早日完善，也希望这篇文章能够帮到路过的你~








