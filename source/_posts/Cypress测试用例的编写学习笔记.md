---
title: Cypress测试用例的编写学习笔记
tags:
  - Web开发
originContent: ''
categories:
  - 自动化测试
toc: false
date: 2021-09-17 14:29:37
---

## 前言

> cypress 底层依赖于很多优秀的开源框架，其中包含Mocha。mocha是一个适用于Node.js和浏览器的测试框架。它使用异步测试变得简单、灵活和有趣。

## 在Cypress中基于mocha提供的如下基本功能模块
- describe()
- context()
- in()
- before()
- beforeEach()
- afterEach()
- after()
- .only()
- .skip()

## 下面我们来说下每个函数
- describe()
describe(name, function(){}) 可以理解为一个模块，在模块内可以下嵌套多个it() 参数name为字符串可以理解为测试用例集描述，function（）用于执行步骤。

- it()
用于声明一个测试用例，describe()套件中必须至少含有一个it()。

## 钩子函数用法

- before(）初始化执行所有用例之前运行，执行一次
- beforeEach() 每条用例执行之前都执行
- afterEach() 每条用例执行之后都执行
- after() 初始化执行所有用例完之后运行，执行一次

```js
/**
 * Create by dell on 2020/6/6
 * 作者 ：wencheng
 * */


describe('登陆web网站案列', function () {
	before(function () {
		//初始化执行所有用例之前运行，执行一次
		cy.log("我是before")
	})
	after(function () {
		//初始化执行所有用例完之后运行，执行一次
		cy.log("我是after")
	})
	beforeEach(function () {
		//每条用例执行之前都执行
		cy.log("我是beforeEach")
	})
	afterEach(function () {
		//每条用例执行之后都执行
		cy.log("我是afterEach")
	})
	it('钩子函数测试01', function () {
		cy.log("hello cypress")
	})
	it('钩子函数测试02', function () {
		cy.log("hello cypress")
		
	})
})
```
执行结果：

![image.png](https://blogimage.houjiyi.com/FvXUrth1k_niae2GG6KNFYw1WJ29)

## .skip()用于跳过不需要执行的测试集合describe()或者测试用例it()

跳过describe()

```js
/**
 * Create by dell on 2020/6/6
 * 作者 ：wencheng
 * */



describe.skip('skip1', function () {
	it('skip函数测试01', function () {
		cy.log("hello cypress")
	})
})

describe('skip12', function () {
	it('skip函数测试01', function () {
		cy.log("hello cypress")
	})
})
```

执行结果可以看出第一个describe()被忽略了

![image.png](https://blogimage.houjiyi.com/FnF47D1mfNmvaeWBL18vZrEWHzPk)

跳过it()

```js
/**
 * Create by dell on 2020/6/6
 * 作者 ：wencheng
 * */

describe('skip3', function () {
	it.skip('skip函数测试01', function () {
		cy.log("hello cypress")
	})
	it('skip函数测试02', function () {
		cy.log("hello cypress")
	})
})
```

执行结果可以看出第一个it()被忽略了


.only指定要运行的测试模块describe()和测试用例it()

指定要执行的测试模块describe.only()

```js
/**
 * Create by dell on 2020/6/6
 * 作者 ：wencheng
 * */


describe.only('only1', function () {
	it('only函数测试01', function () {
		cy.log("hello cypress")
	})
})

describe('only2', function () {
	it('only函数测试02', function () {
		cy.log("hello cypress")
	})
})
```

执行结果可以看到只有第一个测试用例集被执行了，第二个没执行。
![image.png](https://blogimage.houjiyi.com/Fv0IQJ2pMHTzzPG3oQuF5TGhthzx)

指定要执行的测试用例it().only()

```js
/**
 * Create by dell on 2020/6/6
 * 作者 ：wencheng
 * */


describe('only3', function () {
	it.only('only函数测试01', function () {
		cy.log("hello cypress")
	})
	it('only函数测试02', function () {
		cy.log("hello cypress")
	})
})
```

执行结果可以看到只有第一个测试用例被执行了，第二个没执行。

![image.png](https://blogimage.houjiyi.com/FgYKvlEHDZgUpmIJPQVWJHs9NhvE)

- 动态使用.skip函数跳过用例

根据判断来进行

```js
/**
 * Create by dell on 2020/6/6
 * 作者 ：wencheng
 * */


describe('skip_Dynamic', function () {
	const a = 1
	it('skip函数测试', function () {
		if (a == 1) {
			cy.log("hello cypress")
		} else {
			cy.log("skip cypress")
			this.skip()
		}
	})
	it('skip函数测试', function () {
		if (a == 0) {
			cy.log("hello cypress")
		} else {
			cy.log("skip cypress")
			this.skip()
		}
	})
})
```
执行结果
![image.png](https://blogimage.houjiyi.com/FsoBcB0m7YJdCJ3LXNsPXkwJiPgU)

以上就是这篇文章的全部内容了，希望本文的内容对大家的学习或者工作具有一定的参考学习价值，如果有疑问大家可以留言交流，谢谢大家的支持。