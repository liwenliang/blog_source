---
title: Cypress 可操作事件
tags:
  - Web开发
categories:
  - 自动化测试
toc: false
date: 2021-09-17 13:57:44
---

- click()
![image.png](https://blogimage.houjiyi.com/FnGC0_r2pbgwf072BWHK8xhoYvjJ)

单击DOM元素语法如下：

```js
cy.get('.action-btn').click()
// clicking in the center of the element is the default
cy.get('#action-canvas').click()

cy.get('#action-canvas').click('topLeft')
cy.get('#action-canvas').click('top')
cy.get('#action-canvas').click('topRight')
cy.get('#action-canvas').click('left')
cy.get('#action-canvas').click('right')
cy.get('#action-canvas').click('bottomLeft')
cy.get('#action-canvas').click('bottom')
cy.get('#action-canvas').click('bottomRight')

// .click() accepts a an x and y coordinate
// that controls where the click occurs :)
cy.get('#action-canvas')
  .click(80, 75)
  .click(170, 75)
  .click(80, 165)
  .click(100, 185)
  .click(125, 190)
  .click(150, 185)
  .click(170, 165)

// click multiple elements by passing multiple: true
cy.get('.action-labels>.label').click({ multiple: true })

// Ignore error checking prior to clicking
cy.get('.action-opacity>.btn').click({ force: true }
```

- dblclick()
双击DoM元素语法如下:
```js
//双击DoM元素
dblclick()
//带参数的双击
dblclick(options)
```
带参数跟上面click完全一致

- rightclick()
右击DoM元素语法如下:
```js
//右击DoM元素
rightclick()
//带参数的双击
rightclick(options)
```

带参数跟上面click完全一致

- type()

往DOM元素中输入

type()语法如下:

```js
//输入文本
type(text)
//输入文本带参数的
rightclick(text,options)
```

text参数支持的其他如下字符如下：

1. {backspace} ：删除光标左侧的字符
2. {det} : 删除光标右侧的字符
3. {downarrow} : 向下移动光标
4. {end} : 将光标移到行尾
5. {enter} ：按enter建
6. {esc} : 按esc建
7. {home} ： 将光标移到行首
8. {insert} ：在光标右侧插入字符
9. {leftarrow} ：向左移动光标
10. {pagedowm} ：向下滚动
11. {pageup} ：向下滚动
12. {rigtharrow} ：向右移动光标
13. {selectall} ：通过选择范围来选择所有文本
14. {uparrow} ：向上移动光标

- clear()
clear()清除输入或文本区域的值，语法如下：
```js
//清除
cy.get().clear()
```

- check()
针对\<input\>类型的输入框（radio button） 或者复选框（check box）Cypree提供了check和uncheck方法直接操作。语法如下：
```js 
//选中
.check()
//选中一个选项，值value
.check(value)
//选中多个选项
.check(values)
```

- uncheck()

uncheck()和check用法相反用于取消单选框或者复选框语法参考check()

- .selelct()
select()用来在\<select\>中选中一个\<option\>。语法如下
```js
.select(value)
```

- .trigger()
用来在DOM元素上触发事件。用法如下：

.trigger(evenName)

```javascript
//按下光标
cy.button().trigger('mousedown')
//移动光标到元素之上
cy.button().trigger('mouseover')
//抬起光标
cy.button().trigger('mouseleave')
```

以上就是这篇文章的全部内容了，希望本文的内容对大家的学习或者工作具有一定的参考学习价值，如果有疑问大家可以留言交流，谢谢大家的支持。