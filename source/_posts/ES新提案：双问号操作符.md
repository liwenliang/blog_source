---
title: ES新提案：双问号操作符
tags:
  - 知识点备忘
  - 基本知识
  - javascript
originContent: ''
categories:
  - Web开发
toc: false
date: 2021-12-29 18:51:05
---

本文主要讲Gabriel Isenberg撰写的ES提案“Nullish coalescing for JavaScript”。 它提出?? 替换||的运算符,并提供默认值。这里先把这相提案叫作双问号操作符，如果你有好的叫法，欢迎留言讨论。

## 1. 概述
双问号 ?? 的操作符跟 || 类似，如果给定变量值为 null 或者 undefined，刚使用双问号后的默认值，否则使用该变量值。

如下:

```javascript
> undefined ?? 'default'
'default'
> null ?? 'default'
'default'
> false ?? 'default'
false
> '' ?? 'default'
''
> 0 ?? 'default'
0
```

## 2. 早期的 || 运算符号
直接来个例子来演示一下 || 运算，下面两个等式是等价的：
```javascript
a || b
a ? a : b
```

如果 a 是 truthy 值，则返回 a, 否则返回 b。

这使得使用||指定一个默认值成为可能，如果实际值是假的，那么将使用这个默认值：

```javascript
const result = actualValue || defaultValue;
function getTitle(fileDesc) {
  return fileDesc.title || '(Untitled)';
}
const files = [
  {path: 'index.html', title: 'Home'},
  {path: 'tmp.html'},
];
assert.deepEqual(
  files.map(f => getTitle(f)),
  ['Home', '(Untitled)']);
```

请注意，基本只有在实际值undefined或为null时才应使用默认值，这是有效的，因为undefined和null都是假(虚值)的：

```javascript
> undefined || 'default'
'default'
> null || 'default'
'default'
```

遗憾的是，如果实际值是其他的虚值，也会使用默认值：

```javascript
> false || 'default'
'default'
> '' || 'default'
'default'
> 0 || 'default'
'default'
```

因此，这个getTitle()并不总能正常工作：

```javascript
assert.equal(
  getTitle({path: 'empty.html', title: ''}),
  '(Untitled)');
```

## 3. 使用双问号操作符来解决 || 运算的问题


?? 主要是用来解决 || 操作符号的一些问题，以下两个表达式是等价的：

```javascript
a ?? b
a !== undefined && a !== null ? a : b
```

默认值是这样提供的：

```javascript
const result = actualValue ?? defaultValue;

```

对于undefined和null， ??操作符的工作原理与||操作符相同

```javascript
> undefined ?? 'default'
'default'
> null ?? 'default'
'default'

```

除了 undefined 和 null的其它虚值，?? 不会返回默认值。

```javascript
> false ?? 'default'
false
> '' ?? 'default'
''
> 0 ?? 'default'
0
```

使用 ?? 来重写 getTitle():

```javascript
function getTitle(fileDesc) {
  return fileDesc.title ?? '(Untitled)';
}
```

现在使用fileDesc调用它，它的.title是空字符串，仍然可以按符合咱们的预期工作：

```javascript
assert.equal(
  getTitle({path: 'empty.html', title: ''}),
  '');
```

### 3.1 通过解构给定默认值
除了使用 ?? 给getTitle添加默认值，咱们也可以通过解构方式来给定默认值：

```javascript
function getTitle({title = '(Untitled)'}) {
  return title;
}
```
### 3.2 使用 ?? 操作符号的实际例子
作为一个现实的例子，咱们使用??来简化下面的函数。

```javascript
function countMatches(regex, str) {
  if (!regex.global) {
    throw new Error('Regular expression must have flag /g: ' + regex);
  }
  const matchResult = str.match(regex); // null or Array
  if (matchResult === null) {
    return 0;
  } else {
    return matchResult.length;
  }
}

assert.equal(
  countMatches(/a/g, 'ababa'), 3);
assert.equal(
  countMatches(/b/g, 'ababa'), 2);
assert.equal(
  countMatches(/x/g, 'ababa'), 0);

// Flag /g is missing
assert.throws(
  () => countMatches(/a/, 'ababa'), Error);
```
使用 ?? 操作符号后，简化如下：

```javascript
function countMatches(regex, str) {
  if (!regex.global) {
    throw new Error('Regular expression must have flag /g: ' + regex);
  }
  return (str.match(regex) ?? []).length;
}

```
### 3.3 双问号(??)操作符与可选链(?)
双问号(??)的提出是为了补充可选链(?)，来看看这两兄弟结合使用的场景（第A行）：

```javascript
const persons = [
  {
    surname: 'Zoe',
    address: {
      street: {
        name: 'Sesame Street',
        number: '123',
      },
    },
  },
  {
    surname: 'Mariner',
  },
  {
    surname: 'Carmen',
    address: {
    },
  },
];

const streetNames = persons.map(
  p => p.address?.street?.name ?? '(no name)'); // (A)
assert.deepEqual(
  streetNames, ['Sesame Street', '(no name)', '(no name)']
);
```
## 4. 兼容性
可以通过ECMAScript Next compatibility table 查看 ?? 支持情况。






