---
title: Cypress录制自动化脚本
tags:
  - Web开发
categories:
  - 自动化测试
toc: false
date: 2021-09-17 14:37:58
---

# 1. 前言
Cypress Studio提供了一种在测试运行程序中生成测试的可视化方法，通过记录与被测应用程序的交互。支持.click（）、.type（）、.check（）、.uncheck（）和.select（）Cypress命令，这些命令将在与Cypress Studio内部的DOM交互时生成测试代码。

# 2. 使用Cypress Studio
Cypress Studio是一个实验性功能，可以通过向配置文件添加experimentalStudio属性来启用(塞浦路斯.json默认情况下）。
```json
{
  "experimentalStudio": true
}
```

我们以登录案例，以演示Cypress测试方法、模式和工作流的真实使用。它将用于演示下面Cypress Studio的功能。

# 3.扩展测试
您可以扩展任何先前存在的测试，也可以使用以下测试支架在您的默认情况下`integrationFolder`（`cypress/integration`在默认情况下）创建一个新测试来开始。
```js
describe('第一个录制脚本从禅道登录开始', function () {
    beforeEach(() => {
        cy.visit('http://localhost:8080/zentao/user-login.html')
    })
    it("登录输入框功能", function () {
    })
})
```

## 1. 运行规范
我们将使用Cypress Studio执行“新事务”用户旅程。首先，启动测试运行程序并运行在上一步中创建的规范。
![image.png](https://blogimage.houjiyi.com/FsuEGRfK4OXzF8LWcJCH3xd93O8w)

测试完成运行后，将鼠标悬停在命令日志中的测试上方，以显示“将命令添加到测试”按钮。单击“添加要测试的命令”将启动Cypress Studio。
![image.png](https://blogimage.houjiyi.com/FvmapFIa7x1YhSDOpAIlYtH9sVQU)

## 2. 启动Cypress Studio
![image.png](https://blogimage.houjiyi.com/FkhCu1daUFlpDuVabCFJV5JNNxOA)
现在，我们可以开始更新测试以在用户之间创建新事务。

## 3.应用程式互动
要记录操作，请开始与应用程序进行交互。在这里，我们将单击账号密码输入框，结果将看到单击记录在命令日志中。
![image.png](https://blogimage.houjiyi.com/FmIfiFLXAaRQMwNFaxxo2HaUlZab)

要放弃交互，请单击“取消”按钮退出Cypress Studio。如果对与应用程序的交互感到满意，请单击“保存命令”，测试代码将保存到spec文件中。

## 4. 生成的测试代码
查看我们的测试代码，我们可以看到单击“ Save Commands”（保存命令）后更新了测试，并使用了我们在Cypress Studio中记录的操作。

```js
describe('第一个录制脚本从禅道登录开始', function () {
    it("登录输入框功能", function () {
        cy.visit('http://localhost:8080/zentao/user-login.html')

        /* ==== Generated with Cypress Studio ==== */
        cy.get('#account').type('admin');
        cy.get('.col-8').click();
        cy.get(':nth-child(2) > td > .form-control').type('longshi@2020');
        cy.get('.form-actions').click();
        cy.get('#submit').click();
        /* ==== End Cypress Studio ==== */
    })
})
```

## 5. 添加新测试
您可以通过在我们定义的块上单击“添加新测试”，将新测试添加到任何现有describe或块中。context``describe
![image.png](https://blogimage.houjiyi.com/Fl5x0MTiuC2GeZ4OeMVS1KGWIBEp)

保存后，该文件将在cypress中再次运行。
![image.png](https://blogimage.houjiyi.com/FiPk98lQwZvtFWVkCqaIZa6AgioG)

最后，查看我们的测试代码，我们可以看到单击“ Save Commands”（保存命令）后更新了测试，并使用了我们在Cypress Studio中记录的操作。

```js
describe('第一个录制脚本从禅道登录开始', function () {
    it("登录输入框功能", function () {
        cy.visit('http://localhost:8080/zentao/user-login.html')

        /* ==== Generated with Cypress Studio ==== */
        cy.get('#account').type('admin');
        cy.get('.col-8').click();
        cy.get(':nth-child(2) > td > .form-control').type('longshi@2020');
        cy.get('.form-actions').click();
        cy.get('#submit').click();
        /* ==== End Cypress Studio ==== */
    })

    /* === Test Created with Cypress Studio === */
    it('TestDemo', function() {
        /* ==== Generated with Cypress Studio ==== */
        cy.visit('http://localhost：8080/zentao/user-login.html');
        cy.get('#account').type('admin');
        cy.get('#login').click();
        cy.get(':nth-child(2) > td > .form-control').type('longshi@2020');
        cy.get('#submit').click();
        cy.get('[data-id="product"] > a').click();
        /* ==== End Cypress Studio ==== */
    });
})
```

## 4. 插件Cypress Recorder
通过Cypress Recorder也可以实现录制脚本
百度网盘下载：

> 链接: https://pan.baidu.com/s/1_e-Js8vEugsmNiajOySvXg  
> 提取码: 5mse 

### 1. Cypress Recorder安装
将下载的zip解压本地，加入到chrome浏览器扩展程序中
![image.png](https://blogimage.houjiyi.com/Fmeh_C7RTAK2zYaVj3QOw3TQ-6FV)

### 2.启动Cypress Recorder
![image.png](https://blogimage.houjiyi.com/Fnl69j3WCagkMALTwDt4AII6cY38)

### 3.点击 Start Recording
![image.png](https://blogimage.houjiyi.com/FukM0wMgFKRvzi2yX1H1U6hvxTor)

```js
cy.visit('https://www.baidu.com/');
cy.get('#lg').click();
cy.get('#kw').click();
cy.get('#kw').type('博客园');
cy.get('#su').click();
cy.get('#form').submit();
```

以上就是这篇文章的全部内容了，希望本文的内容对大家的学习或者工作具有一定的参考学习价值，如果有疑问大家可以留言交流，谢谢大家的支持。