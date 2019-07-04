---
title: export default 和 export 区别
tags:
  - javascript
originContent: ''
categories:
  - Web开发
toc: false
date: 2019-07-04 13:08:10
---

1. export 与 export default 均可用于导出常量、函数、文件、模块等
2. 你可以在其它文件或模块中通 import 导入
3. 在一个文件或模块中，export、import可以有多个，export default仅有一个
4. 通过 export 方式导出，在导入时要加{ }，export default 则不需要

```javascript
// export
export class Login extends Component {...}
import {Login} from './login';

//export default
export default class Home extends Component {...}
import Home from './home';
```
