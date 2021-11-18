---
title: Cypress 常见操作
tags:
  - Web开发
categories:
  - 自动化测试
toc: false
date: 2021-09-17 13:52:36
---

- 访问某个link
```javascript
//访问百度
cy.visit('httpf://www.baidu.com)
```

- 获取当前页面URL
```javascript
//获取页面地址
cy.url()
cy.url().should('contain','link')
```

- 刷新页面
```javascript
// 等同于F5
cy.reaload()
// 等同于ctrl+F5强制刷新
cy.radload(true)
```

- 设置窗口
```javascript
//在cypress.json中添加
{ 
   'viewportWidth':'1000',
   'viewportHeight':'600'
}
//运行中设置
cy.viewpoint(1024,768)
```

- 前进后退
```js
//后退
cy.go('back)
cy.go(-1)
//前进
cy.go('forward)
cy.go(1)
```

- 判断元素是否存在
```js
//判断check-box是否可见
cy.get('.check-box).should('be.visible')
//判断元素存在
cy.get('.check-box).should('exist')
//判断元素不存在
cy.get('.check-box).should('no exist')
```

- 条件判断
```js
//利用jquery来判断元素是否存在
const btn = '#btn'
Cypress.$(btn).length>0{
 cy.get(btn).click()
}
```

- 获取元素属性值
```js
//获取元素btn的文本
cy.get('#btn').then(function(){
   const btnTxt = $btn.text()
   cy.log(btnTxt)
})
```

- 清除文本
```js
//清除input输入的值
cy.get('div>a').clear()
cycy.get('div>a').clear().type()
```

- 操作单选/多选按钮
```js
//选中
cy.get('radio').check('us)
//取消选中
cy.get('radio').uncheck('us)
```

- 操作下拉菜单
```js
cy.get('select').select('下拉选项的值')
cy.get('li').eq(0).click()
```

- 操作弹出框
```js
cy.get('iframe')
    .then(function($iframe){
     //定义要查找的元素
     const $body = $iframe.contents().find('body')
     //在查找到的元素中查找btn并单击
     cy.wrap($body).find('#bin').click()
})
```

- 操作被覆盖带元素
```js
cy.get('#btn').click({force:true})
```

- 模拟键盘操作
```js
cy.get('input').type('111')
cy.get('input').type('{enter}')
```

以上就是这篇文章的全部内容了，希望本文的内容对大家的学习或者工作具有一定的参考学习价值，如果有疑问大家可以留言交流，谢谢大家的支持。