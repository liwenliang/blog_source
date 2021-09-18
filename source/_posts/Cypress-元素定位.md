---
title: Cypress 元素定位
tags:
  - Web开发
originContent: ''
categories:
  - 自动化测试
toc: false
date: 2021-09-17 12:33:34
---

前言
Cypress含有多种定位方式我们无需担心因为定位导致测试失败，Cypress有独一无二的定位策略能使你摆脱元素定位的噩梦。

Cypress专有选择器

- data-cy
- data-test
- data-testid


举例：
```html
<button id='main' class = 'btn' data-cy = 'submit'>submit<button><button id='main' class = 'btn' data-test= 'submit'>submit<button><button id='main' class = 'btn' data-testid= 'submit'>submit<button>
```
获取元素方法：

```javascript
cy.get('[data-cy=submit]').click()
cy.get('[data-test=submit]').click()
cy.get('[data-testid=submit]').click()
```

举例：
![image.png](https://blogimage.houjiyi.com/FsKusJefzm2L4lb709CcCjijV4ND)
```html
<tbody>
    <tr>
    <th>用户名</th>
    <td><input class="form-control" type="text" name="account" id="account" autofocus=""></td>
    </tr>
    <tr>
    <th>密码</th>
    <td><input class="form-control" type="password" name="password"></td>
    </tr>
    <tr>
    <th></th>
    <td id="keeplogin"><div class="checkbox-primary"><input type="checkbox" name="keepLogin[]" value="on" id="keepLoginon"> <label for="keepLoginon">保持登录</label></div></td>
    </tr>
    <tr>
    <td></td>
    <td class="form-actions">
    <button type="submit" id="submit" class="btn btn-primary" data-loading="稍候...">登录</button><input type="hidden" name="referer" id="referer" value="/zentao/"><input type="hidden" name="verifyRand" id="verifyRand" value="1281275466"><a href="/zentao/user-reset.html">忘记密码</a>
    </td>
    </tr>
</tbody>
```

- #d选择器

#id选择器通过html元素id属性来获取DMO

获取用户名input元素方法：

```javascript
cy.get('#account').click()
```

- class类选择器

类选择器通过html元素class属性来获取DMO

获取用户名input元素方法：

```javascript
cy.get('.form-control').click()
```

- attributes属性选择器

类选择器通过html元素class属性来获取DMO

获取用户名input元素方法：

```javascript
cy.get('[input[id = "account"]]').click()
```

- :nth-child(n)选择器


:nth-child(n)选择器匹配属于其父元素的第n个子元素，不论元素的类型。

获取用户名DOM元素方法：
```javascript
cy.get(tbody > tr:nth-child(1) > th')
```

- Cypress.$定位器

Cypress可以使用jQuery选择器定位

```javascript
Cypress.$('#account') 等价于 cy.get('#account')
```


以上就是这篇文章的全部内容了，希望本文的内容对大家的学习或者工作具有一定的参考学习价值，如果有疑问大家可以留言交流，谢谢大家的支持。
