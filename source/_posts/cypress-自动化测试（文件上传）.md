---
title: cypress 自动化测试（文件上传）
tags:
  - Web开发
originContent: ''
categories:
  - 自动化测试
toc: false
date: 2021-09-17 14:52:04
---

前言
cypress-file-upload插件使得文件上传测试变得简单。这个包添加了一个定制的Cypress命令，允许您对如何通过HTML控件上传文件进行抽象，并将重点放在测试用户工作流上。

1.官方地址
下载地址:

 https://github.com/abramenal/cypress-file-upload 

插件使用详情可查考：

https://www.npmjs.com/package/cypress-file-upload#it-isnt-working-what-else-can-i-try

2.安装插件
npm
```bash
npm install --save-dev cypress-file-upload
```

3.导入插件
您需要将其添加`cypress/support/commands.js`如下
```js
import 'cypress-file-upload';
```

然后，确保将commands.js导入`cypress/support/index.js`（可能会被注释）：
```js
import  './commands' ;
```

4.使用方法
通常的做法是将Cypress测试所需的所有文件放在`Cypress/fixtures`文件夹中。

例如:新建test.txt文件


测试demo
```js
/**
 * Create by dell on 2021/3/20
 * Author ：wencheng
 * 微信公众 ：自动化测试 To share
 * */

describe('file_upload_test',function () {
    beforeEach(() => {
        cy.login_auto()
    })
    it('upload', function() {
        /* ==== Generated with Cypress Studio ==== */
        cy.visit('/zentao/qa/');
        cy.get('[data-id="bug"] > a').click();
        cy.get('p > .btn').click();
        /* ==== End Cypress Studio ==== */
        cy.get("input[type='file']").attachFile('test.txt');
    });
})
```

运行结果上传成功
![image.png](https://blogimage.houjiyi.com/Fp23dAS_B3cGKYKTDm9X0B1AgVyo)

5.注意事项
1.定位上传文件的dom
切记定位的是 `input[type='file']
![image.png](https://blogimage.houjiyi.com/Fp0NLcvG_-mmCFta1wZ67BvrgukW)

2.上传文件内容不能为空
当上传文件内容为空时，文件上传失败我们通过debug看下。
![image.png](https://blogimage.houjiyi.com/Fqg_9-1t-yOSY-En31giVhmatAmN)

未触发文件上传
![image.png](https://blogimage.houjiyi.com/FnU3iJoTDn5l90PBKA8pFWRsN7SX)

触发文件上传会预加载file内容
![image.png](https://blogimage.houjiyi.com/FuG6gDoBMYMgFP-WPIlhmyQuiVx3)

以上就是这篇文章的全部内容了，希望本文的内容对大家的学习或者工作具有一定的参考学习价值.