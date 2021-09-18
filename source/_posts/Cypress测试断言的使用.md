---
title: Cypress测试断言的使用
tags:
  - Web开发
originContent: ''
categories:
  - 自动化测试
toc: false
date: 2021-09-17 14:12:35
---

## 前言

断言是测用例的必要组成部分，Cypress支持多种断言，其中包括BDD（expect/should）和TDD（assert）格式断言。

### 常见的断言方式

- 针对长度（length）的断言
```js
//重试,直到找到3个匹配的<li.selected>
cy.get('li.selected').should('have.length',3)
```

- 针对对类（Class）的断言
```js
//重试,直到input元素没有类被disabled为止（或者超时为止）
cy.get('from').fijd('input').should('not.have.class','disabled')
```

- 针对值（Value）断言
```js
//重试,直到textarea的值为‘iTesting’
cy.get('textarea').should('have.value','iTesting')
```

- 针对文本内容（Text Content）的断言
```js
//重试,直到这个span不包含“click me”字样
cy.get('a').parent('span.help').should('not.contain','click me')
//重试,直到这个span包含“click me”字样
cy.get('a').parent('span.help').should('contain','click me')
```

- 针对元素可见与否（Visibility）的断言
```js
//重试,直到这个button是可为止
cy.get('button').should('be.visible')
```

- 针对元素存在与否（Existence）的断言
```js
//重试,直到id为loading的spinner不在存在
cy.get('#loading').should('not.exist')
```

- 针对元素状态的（State）的断言
```js
//重试,直到这个radio button是选中状态
cy.get('：radio').should('be.checked')
```

- 针对CSS的断言
```js
//重试,直到completed这个类有匹配的css为止
cy.get('.completed').should('have.css','text-decoration','line-through')
```
更多的断言库及其用法，请参考如下网站：

https://github.com/chaijs/chai​github.com

https://github.com/domenic/sinon-chai​github.com

https://github.com/chaijs/chai-jquery​github.com

Assert - Chai​www.chaijs.com