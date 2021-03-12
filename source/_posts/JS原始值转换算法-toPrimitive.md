---
title: JS原始值转换算法---toPrimitive()
tags:
  - 知识点备忘
  - javascript
originContent: ''
categories:
  - Web开发
toc: false
date: 2020-07-16 10:58:44
---

在js中，想要将对象转换成原始值，必然会调用toPrimitive()内部函数，那么它是如何工作的呢？

该函数形式如下：

```javascript
toPrimitive(input,preferedType?)
```

input是输入的值，preferedType是期望转换的类型，他可以是字符串，也可以是数字。

如果转换的类型是number，会执行以下步骤：

1. 如果input是原始值，直接返回这个值；
2. 否则，如果input是对象，调用input.valueOf()，如果结果是原始值，返回结果；
3. 否则，调用input.toString()。如果结果是原始值，返回结果；
4. 否则，抛出错误。

如果转换的类型是String，2和3会交换执行，即先执行toString()方法。

你也可以省略preferedType，此时，日期会被认为是字符串，而其他的值会被当做Number。

综上所述，会有以下计算结果：
```javascript
[]+[]
""
```

加号操作符会将preferedType看成Number，调用ES内部的toPrimitive(input，Number)方法，得到空字符串

```javascript
[]+{}
"[object Object]"
```
 最终会调用双方的toString()方法，再做字符串加法
```javascript
{}+[]
0
```
但是空对象加空数组就不一样了，加号运算符的定义是这样的：如果其中一个是字符串，另一个也会被转换为字符串，否则两个运算数都被转换为数字。而同时，javascript有这样的特性，如果{}既可以被认为是代码块，又可以被认为是对象字面量，那么js会把他当做代码块来看待。

这就很好解释了，{}被当做了代码块，只有+[]，根据加法的定义，被转换为0，就得到了结果。

在操作符中，==，排序运算符，加减乘除，在对非原始值进行操作时，都会调用内部的toPrimitive()方法