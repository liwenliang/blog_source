---
title: inquirer.js —— 一个用户与命令行交互的工具
tags:
  - nodejs
categories:
  - Web开发
toc: false
date: 2022-01-18 10:40:03
---

## 写在前面：
开始通过npm init 创建package.json的时候就有大量与用户的交互(当然也可以通过参数来忽略输入)；而现在大多数工程都是通过脚手架来创建的，使用脚手架的时候最明显的就是与命令行的交互，如果想自己做一个脚手架或者在某些时候要与用户进行交互，这个时候就不得不提到inquirer.js了。

## 零. 介绍
由于交互的问题种类不同，inquirer为每个问题提供很多参数：

- type：表示提问的类型，包括：input, confirm, list, rawlist, expand, checkbox, password, editor；
- name: 存储当前问题回答的变量；
- message：问题的描述；
- default：默认值；
- choices：列表选项，在某些type下可用，并且包含一个分隔符(separator)；
- validate：对用户的回答进行校验；
- filter：对用户的回答进行过滤处理，返回处理后的值；
- transformer：对用户回答的显示效果进行处理(如：修改回答的字体或背景颜色)，但不会影响最终的答案的内容；
- when：根据前面问题的回答，判断当前问题是否需要被回答；
- pageSize：修改某些type类型下的渲染行数；
- prefix：修改message默认前缀；
- suffix：修改message默认后缀。

> 上面的属性(除transformer外)在下面都有对应使用。

## 一. 使用

### 0. 语法结构
```javascript
const inquirer = require('inquirer');

const promptList = [
    // 具体交互内容
];

inquirer.prompt(promptList).then(answers => {
    console.log(answers); // 返回的结果
})
```
效果：
![image.png](https://blogimage.houjiyi.com/FpCKD8ILrREeyTTFXny-oyrHeKDZ)

### 1. input
```javascript
const promptList = [{
    type: 'input',
    message: '设置一个用户名:',
    name: 'name',
    default: "test_user" // 默认值
},{
    type: 'input',
    message: '请输入手机号:',
    name: 'phone',
    validate: function(val) {
        if(val.match(/\d{11}/g)) { // 校验位数
            return true;
        }
        return "请输入11位数字";
    }
}];
```
效果：
![image.png](https://blogimage.houjiyi.com/FpCKD8ILrREeyTTFXny-oyrHeKDZ)

### 2. confirm
```javascript
const promptList = [{
    type: "confirm",
    message: "是否使用监听？",
    name: "watch",
    prefix: "前缀"
},{
    type: "confirm",
    message: "是否进行文件过滤？",
    name: "filter",
    suffix: "后缀",
    when: function(answers) { // 当watch为true的时候才会提问当前问题
        return answers.watch
    }
}];
```
效果：
![image.png](https://blogimage.houjiyi.com/Fi3aKc05lrwNw9GcyPOwQY4EMyCf)

![image.png](https://blogimage.houjiyi.com/FhRJQBDhImOxinB1o8cCZzy4twph)

### 3. list

```javascript
const promptList = [{
    type: 'list',
    message: '请选择一种水果:',
    name: 'fruit',
    choices: [
        "Apple",
        "Pear",
        "Banana"
    ],
    filter: function (val) { // 使用filter将回答变为小写
        return val.toLowerCase();
    }
}];
```
效果：

![image.png](https://blogimage.houjiyi.com/FlAakepYOGFZRnpCbQAQHMMyKGJD)

![image.png](https://blogimage.houjiyi.com/Fp7NWFvC268cc-YRvikaAcW3mCjF)

### 4. rawlist
```javascript
const promptList = [{
    type: 'rawlist',
    message: '请选择一种水果:',
    name: 'fruit',
    choices: [
        "Apple",
        "Pear",
        "Banana"
    ]
}];
```
效果：

![image.png](https://blogimage.houjiyi.com/FuRun4P5FKDtg-qk-JBpnEWkUuXq)

### 5. expand

```javascript
const promptList = [{
    type: "expand",
    message: "请选择一种水果：",
    name: "fruit",
    choices: [
        {
            key: "a",
            name: "Apple",
            value: "apple"
        },
        {
            key: "O",
            name: "Orange",
            value: "orange"
        },
        {
            key: "p",
            name: "Pear",
            value: "pear"
        }
    ]
}];
```
效果：

![image.png](https://blogimage.houjiyi.com/FmiD_gtEsgL1x-YX2mLs5uQTUftj)

![image.png](https://blogimage.houjiyi.com/FlSVA9H_Gqb6wUqo2zwaz5stIR5p)

### 6. checkbox
```javascript
const promptList = [{
    type: "checkbox",
    message: "选择颜色:",
    name: "color",
    choices: [
        {
            name: "red"
        },
        new inquirer.Separator(), // 添加分隔符
        {
            name: "blur",
            checked: true // 默认选中
        },
        {
            name: "green"
        },
        new inquirer.Separator("--- 分隔符 ---"), // 自定义分隔符
        {
            name: "yellow"
        }
    ]
}];
// 或者下面这样
const promptList = [{
    type: "checkbox",
    message: "选择颜色:",
    name: "color",
    choices: [
        "red",
        "blur",
        "green",
        "yellow"
    ],
    pageSize: 2 // 设置行数
}];
```
效果：

![image.png](https://blogimage.houjiyi.com/FlQEg1neWZurjgb5W23IuDrc282x)

![image.png](https://blogimage.houjiyi.com/FsF6jt2iLdoYcmBhuplrp9gZEDRI)

### 7. password
```javascript
const promptList = [{
    type: "password", // 密码为密文输入
    message: "请输入密码：",
    name: "pwd"
}];
```

效果：

![image.png](https://blogimage.houjiyi.com/FqzRGPJO6VB7qd7Zehfc4I7vyrqF)

### 8. editor

```javascript
const promptList = [{
    type: "editor",
    message: "请输入备注：",
    name: "editor"
}];
```
效果：
![image.png](https://blogimage.houjiyi.com/FusoJTvhhZmNfExF4nS7dhgusNtn)

![image.png](https://blogimage.houjiyi.com/FnEnsojR969ChOohJ4clHzKdW-w8)


写在后面：
Everything will be ok in the end, if it’s not ok, it’s not the end.

原文地址：[原文链接](https://blog.csdn.net/qq_26733915/article/details/80461257)