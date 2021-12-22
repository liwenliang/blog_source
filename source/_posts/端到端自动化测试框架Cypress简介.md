---
title: 端到端自动化测试框架Cypress简介
tags:
  - Web开发
categories:
  - 自动化测试
toc: false
date: 2021-12-22 14:52:08
---

## 总揽
> 当我看到Cypress的时候，有一种当初看到vue的时候的感觉：恰好是我需要的，恰好能做到我需要的所有事情，同时也是渐进式无侵入性的。

Cypress有一系列的目标：
1. web已经升级更新了，测试也同样需要升级更新
2. Cypress是一款能够快速、简单、稳定的测试所有运行在浏览器上的内容的端到端测试工具
3. Cypress是一个完整的端到端测试体验框架，他不需要安装其他额外的依赖包
4. 安装Cypress是非常简单的，没有依赖、额外下载同时不需要修改你的代码
5. 测试你的代码而非你的耐心：Cypress是一个新的针对开发人员与QA工程师定制的标准的端到端测试框架

## 测试金字塔模型
![image.png](https://blogimage.houjiyi.com/FlN8l4aLzLawvltNQGAN0jkDJwQC)

## UI自动化测试
UI测试的主要目的是从软件使用者的角度来检验软件的质量，而UI自动化测试则是以自动化的方式来代替人工执行测试。在测试金字塔模型中，UI层测试是各种测试中投入最大、收益最低、运行最慢的一种。

而Cypress的出现实际可以在某种程度上打破这种局面

## 单元测试
### 单元测试有这样一段描述很有意思：
![image.png](https://blogimage.houjiyi.com/Fi3i_V5lomsSicoZ1dAV0Pe4UbLn)

### 单元测试的价值
![image.png](https://blogimage.houjiyi.com/FhtuMLiTAYzZl6aJjrFqrTeKoUPK)

## Cypress简介
- Cypress是为现代网络打造的，基于JavaScript的下一代前端测试工具。他可以对浏览器中运行的任何内容进行快速，简单和可靠的测试。
- Cypress是自集成的，它提供了一套完整的端到端测试体验。无须借助其他外部工具，在简单安装后即可允许用户快速的创建、编写、运行、测试用例，并且针对每一步操作均支持回看。
- 不同于其他只能测试UI层的前端测试工具，Cypress允许你编写所有类型的测试，覆盖了测试金字塔模型涉及的所有测试类型：端到端测试；集成测试；单元测试；

## Cypress优点
- 界面美观友好。
- 支持手机模拟。
- 测试的每一步都有对应的截图。
- 全程都会有录屏。
- 支持debug调试，随时暂停。
- 自动等待ui更新，减少异步代码。

## Cypress带来的改变
有了Cypress，配置测试框架不再是一件看起来非常繁琐的事情，它有你所有想要的功能：
![image.png](https://blogimage.houjiyi.com/FofEllCW9pr0mE68iI_9FiG36iHI)

## Cypress与其他测试框架的比较
|对比维度|Cypress|Selenium/WebDriver|
|-|-|-|
|框架完备|是|否（仅是Library）|
|开箱即用|是|否（无法直接工作）|
|自带断言库|是|否（需自行添加）|
|自带Mock|是|否（需自行添加）|
|自带测试运行器（Test Runner）|是|否（需自行添加）|

|对比维度|Cypress|Karma|
|-|-|-|
|框架完备|是|否（仅是Test Runner）|
|开箱即用|是|否（无法直接工作）|
|自带断言库|是|否（无法直接工作）|
|自带Mock|是|否（无法直接工作）|
|自带测试运行器（Test Runner）|是|否（无法直接工作）|

|对比维度|Cypress|Nightwatch|
|-|-|-|
|框架完备|是|是|
|开箱即用|是|是（安装，配置繁琐）|
|底层框架|自主研发，运行快|基于WebDriver，运行慢|
|适用范围|UI,API,UnitTest均支持|仅用于UI测试|
|脚本易维护|是|否（难以维护）|
|回放测试执行情况|是|否|
|多浏览器支持|支持|支持|

|对比维度|Cypress|Protractor|
|-|-|-|
|框架完备|是|是|
|开箱即用|是|是|
|底层框架|自主研发，运行快|基于WebdriverJS，运行慢|
|单元测试框架|底层使用Mocha，不可替换|默认使用Jasmine，允许替换|
|适用范围|多种语言开发的程序均可进行测试|主要应用于Angular的程序|
|回放测试执行情况|是|否|
|多浏览器支持|支持|支持|

|对比维度|Cypress|TestCafe|
|-|-|-|
|框架完备|是|是|
|开箱即用|是|是|
|底层框架|自主研发，运行快|自主研发，通过Proxy Server工作|
|测试用例组织便捷度|基于Mocha，组织、编写简单|测试用例写法异于常规（fixture），学习成本高|
|自带测试运行器|是|是（也仅仅是TestRunner）|
|回放测试执行情况|是|否|
|多浏览器支持|支持|支持|
|测试运行|测试运行在浏览器中|测试运行在Node.js中，便于设置和清除数据库fixtures|

|对比维度|Cypress|Puppeteer|
|-|-|-|
|框架完备|是|是|
|是否有IDE|是|否|
|适用范围|e2e的完美解决方案|主要用于爬虫和开展快速测试|
|自带断言库|是|否|
|回放测试执行情况|是|否|
|多浏览器支持|支持|支持|

## Cypress的局限

### 长期权衡
- 不建议使用Cypress用于网络爬虫，性能测试之目的。
- Cypress永远不会支持多标签测试。
- Cypress不支持同时打开两个及以上的浏览器。
- 每个Cypress测试用例应遵守同源策略
### 短期折中
- 目前浏览器支持Chrome，Firefox，Microsoft Edge和Electron
- 不支持测试移动端应用
- 针对iframe的支持有限
- 不能在window.fetch上使用cy.route()
- 没有影子DOM支持。
### Cypress作为一款优秀的开源软件，其提供的多个免费功能已经能够满足绝大多数团队和个人的需求。

## 安装Cypress

```bash
npm install cypress --save-dev
```
or
```bash
yarn add cypress --dev
```
then
```bash
npx cypress open
```
![image.png](https://blogimage.houjiyi.com/FsuauSfkHCmao-NsUtfVl_r39f7s)

## Cypress元素定位
![image.png](https://blogimage.houjiyi.com/FnDj-Ojn9N-mZ8qYUq3iE1NMorcI)

## Cypress页面元素基本操作方式
![image.png](https://blogimage.houjiyi.com/FvT4HRxbt2LJh57y4JyoQTHMjYrk)

## Cypress常见操作
![image.png](https://blogimage.houjiyi.com/FjcSIqQUiBSNFQ-YAObdVtDNkKVY)

## cypress可操作事件
![image.png](https://blogimage.houjiyi.com/FkWe5GW7nXZoxTiM_EgFJdDK6U1T)

## Cypress上传文件或图片
Cypress的文件或图片上传可以使用这个插件，使用起来还是很方便的：https://github.com/abramenal/cypress-file-upload

![image.png](https://blogimage.houjiyi.com/FnJxwy9FeoGmwMRU8NYZsU1JXi1H)

![image.png](https://blogimage.houjiyi.com/FmZBLX7TlmsbAOZAS283aEPa8Cd8)

## 使用Cypress界面获取要选择的元素
Cypress界面获取元素方式与控制台定位元素方式很像：
![image.png](https://blogimage.houjiyi.com/FiD_mu7irr4OP-9HLmzdt5QmDTaS)

## 使用Cypress录制测试脚本
通过使用Cypress的界面，可以方便的创建测试用例和录制测试脚本，但实际上我们的场景可能会比较复杂，这时候录制的脚本可能达不到我们的需求，因此这里了解即可，实际情况更多还是需要手动去写一些代码。

## Vue项目中单元测试-组件测试
在vue项目中使用组件测试可以查看 [@cypress/vue](@cypress/vuehttps://www.npmjs.com/package/@cypress/vue ) 这个插件以及[官方文档](https://docs.cypress.io/guides/component-testing/framework-configuration#Vue-2-Vue-CLI)。

## 总结
cypress给我们前端开发工程师一个非常友好的方式进行端到端的测试，测试用例使用javascript编写，对于前端开发来说还是非常友好的，这里介绍了cypress的一些功能，实际上上手也并不复杂，同时官网相关的视频资源也很丰富，希望感兴趣的同学能够动手尝试。