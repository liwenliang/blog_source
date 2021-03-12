---
title: JS中typeof的用法
tags:
  - javascript
categories:
  - Web开发
toc: false
date: 2020-07-10 09:50:47
---

js是一门弱语言，它在声明变量时无需确定变量的类型，js在运行时会自动判断。那么如何判断一个变量的类型呢，js提供了typeof运算符，用来检测一个变量的类型。

1. typeof的语法
typeof是一个运算符，有2种使用方式：typeof(表达式)和typeof 变量名，第一种是对表达式做运算，第二种是对变量做运算。
2. typeof的返回值
typeof运算符的返回类型为字符串，值包括如下几种：
```javascript
'undefined'              --未定义的变量或值
'boolean'                 --布尔类型的变量或值
'string'                     --字符串类型的变量或值
'number'                  --数字类型的变量或值
'object'                    --对象类型的变量或值，或者null(这个是js历史遗留问题，将null作为object类型处理)
'function'                 --函数类型的变量或值
```
<!-- more -->
 3. 简单的示例

```javascript
console.log(typeof a);    //'undefined'
console.log(typeof(true));  //'boolean'
console.log(typeof '123');  //'string'
console.log(typeof 123);   //'number'
console.log(typeof NaN);   //'number'
console.log(typeof null);  //'object'    
var obj = new String();
console.log(typeof(obj));    //'object'
var fn = function(){};
console.log(typeof(fn));  //'function'
console.log(typeof(class c{}));  //'function'
```
总结：typeof运算符用于判断对象的类型，但是对于一些创建的对象，它们都会返回'object'，有时我们需要判断该实例是否为某个对象的实例，那么这个时候需要用到instanceof运算符，后续记录instanceof运算符的相关用法。

作者：zh2443
链接：https://www.jianshu.com/p/8107d25f54ac
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。