---
title: Cypress端到端自动化测试学习笔记
tags:
  - Web开发
originContent: ''
categories:
  - 自动化测试
toc: false
date: 2021-09-17 11:46:44
---

前言
一直以来，端到端的测试都是前端开发最头疼的事情。如果没有好的测试工具，一旦需求发生改变，就需要手工测试，费时费力，还会有漏网的 bug。最近接触了一款开箱即用的端到端测试工具——Cypress，真心不错，Cypress可以对在浏览器中运行的任何东西进行快速、简单和可靠的测试。

windows环境安装
1.安装node.js
官网下载地址：

https://nodejs.org/en/download/nodejs.org

下载后一路傻瓜式安装，安装完成后，运行cmd，输入node –v查看版本号，然后输入npm -v 出现如下图说明安装成功
![image.png](https://blogimage.houjiyi.com/Fg2xrY3zEEndo1ypZfjZcoyDQUHf)

2.安装Cypress
自己本地电脑新建一个目录Ui_test，cd 到目录，执行 npm 指令安装

cd D:\workspace\Ui_test

npm install cypress  --save-dev (默认安装最新版)

npm install --save-dev cypress@4.5.0 （指定4.5.0版本）

这里博主安装的是4.5.0版本，安装会很慢，耐心等待会。

出现如下说明安装成功了
![image.png](https://blogimage.houjiyi.com/FkrSmBk8N5LFiuv_l6YylCFGi3ZI)

启动cypress
首先在D:\workspace\UI_test目录下新建一个 package.json 文件配置如下内容
```bash
{
  "scripts": {
    "cypress:open": "cypress open"
  }
}
```
现在，您可以在D:\workspace\Ui_test目下用如下命令启动

npm run cypress:open或者npx cypress open

启动成功展示：
![image.png](https://blogimage.houjiyi.com/Fj-IVIG9QZONZpNZYavqx8zxHDuR)

2.现在我们开始写第一个自动化示例程序：
官方文档：

https://docs.cypress.io/guidesdocs.cypress.io

先说一下cypress的目录结构。cypress共有七个目录，还有一个cypress.json文件
![image.png](https://blogimage.houjiyi.com/Ft4kgPU-46v5BCH2imu4efP3l4oI)

编写测试case
脚本实现功能：

1.输入访问地址

2.获取元素

3.输入账号密码

4.登陆

5.断言

在 integration 目录下新建你的工程目录，如demo，下面拿login.js文件为实例
![image.png](https://blogimage.houjiyi.com/FgMS8UpmF9icCA9QDIQJ5pKnZUA8)

describe 声明一个测试用例集

beforeEach 测试用例前置操作，相当于setup

it声明了一个测试用例

cy.get 定位元素，用css selector定位选择器

should 断言控件含有内容

cy.visit(“/地址”)

case编写完切换到项目根目录执行命令npm run cypress:open或者npx cypress open
![image.png](https://blogimage.houjiyi.com/FrJBEiXzhuuoWF67h7Jyug20xiaW)

运行效果
![image.png](https://blogimage.houjiyi.com/FgnOmLWCz0CwHNI64o7xjuN-dywa)

生成测试报告
npm安装 npm install --save-dev mocha mochawesome mochawesome-merge mochawesome-report-generator

在 cypress.json 设置相关参数：
```bash
{
    "env": {
        "search":"Cypress e2e"
    },
    "reporter": "mochawesome",
    "reporterOptions": {
        "reportDir": "cypress/results",
        "overwrite": false,
        "html": true,
        "json": true
    }
}
```
执行 npx cypress run或者npm run cypress:open

出现如下图说明执行成功了
![image.png](https://blogimage.houjiyi.com/FooN1XsXEbVup7f2naYlN6rKAa5g)

生成html测试报告
![image.png](https://blogimage.houjiyi.com/FvK__kmZnKNn7UKG2M5KUdkR6VPj)

生成xxx.mp4视频
![image.png](https://blogimage.houjiyi.com/Fu4jcwq_BXgnOAbZbXu9l6kBbjmx)

想转行做自动化测试的朋友们，想追求新技术助力公司成长的朋友们，未来5年弯道超车的机会，也许就在今天！

推荐几个好的学习素材

https://github.com/cypress-io/cypressgithub.com

https://github.com/cypress-io/cypress-example-recipes/github.com

https://github.com/cypress-io/cypress-example-kitchensink/github.com