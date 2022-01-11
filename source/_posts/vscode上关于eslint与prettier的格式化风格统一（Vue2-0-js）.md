---
title: vscode上关于eslint与prettier的格式化风格统一（Vue2.0+js）
tags:
  - Web开发
  - vue
  - prettire
  - eslint
categories:
  - Web开发
toc: false
date: 2022-01-11 15:18:15
---

> 通常我们使用VsCode编辑器进行软件开发的时候都会安装prettier和eslint两个插件，插件的作用是为了我们编写出更规范和一致的代码，方便我们后续的开发和维护，但是，有时候我们发现即便是同一个项目下的同学，使用这些插件的方法也是有差异的，这就造成不同的人开发出的代码规范不同，不能说谁对谁错，但这在团队合作中造成了不必要的麻烦，我们有必要遵循一种编码规范，进而避免这种情况的产生，于是我们对现有的规范和成熟的项目进行了梳理对比，总结出一套自认为还算合适的配置，希望能够在项目中帮助到其他人；

## 创建vue项目
我们这里使用相对规范的方式创建一个vue空项目，尽量避免已有项目对我们的干扰

```bash
vue create demo
```
默认项目会帮我们创建babel和eslint，不过我们需要一个没有eslint的环境，因此我们选择`Manually select features`
![image.png](https://blogimage.houjiyi.com/FsfqImUeJ4ZpnDcRx21qHpISpwwz)

我们这里只保留了，Babel，至于Router、Vuex以及其他配置对于我们这里的实例没有影响，可以根据项目需要进行选择，只要保证`Linter / Formatter`没有勾选即可，因为我们后边还会对项目进行Eslint的初始化，这里选上会干扰我们使用。
![image.png](https://blogimage.houjiyi.com/Fpj_1Ay1G8yjYM282l9G6W6xpVaI)

然后是vue版本，我们使用2.x
![image.png](https://blogimage.houjiyi.com/FrC9e69c9RSlODK5Nt6FmkNzWfGf)

剩下的使用默认配置按回车，然后等待项目初始化完成
![image.png](https://blogimage.houjiyi.com/FgH1rtghdVpoYrO7dscCVMIcDH0-)

## esLint初始化
首先这里我们全局安装一下eslint，方便我们使用eslint工具进行初始化`npm install eslint -g`
![image.png](https://blogimage.houjiyi.com/FgnO6MlpVmgOtOSl6mT9UZfu7MZO)

然后进到项目根目录，项目初始结构是这样的，这里可以关注下依赖的包：
![image.png](https://blogimage.houjiyi.com/FkT8FGdr_TaWCrPzT2V_-66_Q-YD)

然后我们执行`eslint --init`，会让我们做一些基础配置，下边是过程中配置非默认的一些截图：
![image.png](https://blogimage.houjiyi.com/Fkq4BaIHtrODfYRay179-bntLqHu)
![image.png](https://blogimage.houjiyi.com/FpmZsZWWESftSQgeFqdiEjz2_BIS)
![image.png](https://blogimage.houjiyi.com/Fj4BpPmWj9P55n10ICCPl6gNNUEW)

初始化过程中，他安装了一些依赖，这些是初始化项目里没有的；
```bash
"eslint": "^7.32.0",
"eslint-config-standard": "^16.0.3",
"eslint-plugin-import": "^2.25.4",
"eslint-plugin-node": "^11.1.0",
"eslint-plugin-promise": "^5.2.0",
"eslint-plugin-vue": "^8.2.0",
```

同时，增加了`.eslintrc.js`文件
```javascript
module.exports = {
  env: {
    browser: true,
    es2021: true
  },
  extends: ['plugin:vue/essential', 'standard'],
  parserOptions: {
    ecmaVersion: 13,
    sourceType: 'module'
  },
  plugins: ['vue'],
  rules: {}
}
```
至此，eslint的安装就结束了，安装完成以后package.json大概是这个样子：
![image.png](https://blogimage.houjiyi.com/FtjtSZn_seL3loNaJ3Mpg5v9Idwg)
后边对项目的改造主要是对.eslintrc.js以及安装它的依赖，以达到我们项目开发规范化的目的；

## 修改配置
准备好我们配置好的文件`.eslintrc.js` `prettier.config.js` `settings.json`

`.eslintrc.js`和`prettier.config.js`分别是eslint和prettier的配置文件；

`setting.json`则是vscode编辑器的配置，这里只需要把里边的配置增加到自己的settings.json配置文件里即可，不需要全部覆盖，因为settings.json还有一些我们自己的个性化配置，这里的配置主要是一些跟代码格式化相关的通用配置；

.eslintrc.js文件：
```javascript
/* eslint-disable */

module.exports = {
  root: true,
  env: {
    browser: true,
    node: true,
    es6: true
  },
  extends: ['plugin:vue/recommended', '@vue/standard'],
  parserOptions: {
    parser: '@babel/eslint-parser',
    sourceType: 'module'
  },
  // 自定义规则，当与基础规则发生冲突时，覆盖基础规则
  // "off" 或 0 - 关闭规则;
  // "warn" 或 1 - 开启规则，使用警告级别的错误;
  // "error" 或 2 - 开启规则，使用错误级别的错误
  rules: {
    // 每行最大属性个数，如果是多行，每行最多一个属性
    'vue/max-attributes-per-line': [
      'error',
      {
        singleline: 10,
        multiline: {
          max: 1
        }
      }
    ],
    // 忽略html标签自闭合 vue中html自终止标签是否写‘/’
    'vue/html-self-closing': 'off',
    // 单行html元素内容在新的一行
    'vue/singleline-html-element-content-newline': 'off',
    // 多行html元素内容在新的一行
    'vue/multiline-html-element-content-newline': 'off',
    // 关闭v-html的校验，因为我们可能会用到富文本编辑器
    'vue/no-v-html': 'off',
    // 一行内容最长有多少字符
    'max-len': ['error', { code: 200 }],
    // 函数名称或function关键字与开始参数之间允许有空格:关 默认为开 与prettier冲突
    'space-before-function-paren': ['error', 'never'],
    // 语句末尾不允许有';'：开
    semi: ['error', 'never'],
    // 不允许使用var
    'no-var': 2,
    // 是否允许promise中 reject()内无内容
    'prefer-promise-reject-errors': ['error', { allowEmptyReject: true }]
    // 不允许对象嵌套对象的大括号之间有空格 例如：{ query: { id: row.id }[这里]}
    // 某些框架下会使用下边的配置，比如elementUI-admin
    // 'object-curly-spacing': [
    //   2,
    //   'always',
    //   {
    //     objectsInObjects: false
    //   }
    // ]
  }
}
```

prettier.config.js文件：
```javascript
module.exports = {
  printWidth: 200, // 设置prettier单行输出（不折行）的（最大）长度

  tabWidth: 2, // 设置工具每一个水平缩进的空格数

  useTabs: false, // 使用tab（制表位）缩进而非空格

  semi: false, // 在语句末尾添加分号

  singleQuote: true, // 使用单引号而非双引号

  jsxSingleQuote: false, // jsx 不使用单引号，而使用双引号

  trailingComma: 'none', // 在任何可能的多行中输入尾逗号

  bracketSpacing: true, // 在对象字面量声明所使用的的花括号后（{）和前（}）输出空格

  jsxBracketSameLine: true, // 在多行JSX元素最后一行的末尾添加 > 而使 > 单独一行（不适用于自闭和元素）

  arrowParens: 'always', // 为单行箭头函数的参数添加圆括号，参数个数为1时可以省略圆括号

  rangeStart: 0, // 只格式化某个文件的一部分

  rangeEnd: Infinity, // 只格式化某个文件的一部分

  filepath: 'none', // 指定文件的输入路径，这将被用于解析器参照

  requirePragma: false, // (v1.7.0+) Prettier可以严格按照按照文件顶部的一些特殊的注释格式化代码，这些注释称为“require pragma”(必须杂注)

  insertPragma: false, //  (v1.8.0+) Prettier可以在文件的顶部插入一个 @format的特殊注释，以表明改文件已经被Prettier格式化过了。

  proseWrap: 'preserve' // (v1.8.2+)
}
```

setting.json文件部分配置：
```json
// 文件：文件自动保存，只要暂停输入
  "files.autoSave": "afterDelay",
  // 文件：文件保存机制：暂停输入3000毫秒即保存
  "files.autoSaveDelay": 3000,
  // 编辑器：这个属性是用来设置建议值的，就是我们在输入co时，编辑器提示我们const这个功能。推荐设置为first——即每次默认选中推荐值第一个，比如你输入co，推荐值为const和constant（按顺序），那么就会选择const。
  // 当然，也推荐设置为recentlyUsedByPrefix，即上次你选择或者输入过什么，这次就默认选中什么，比如你输入co，推荐值为const和constant（按顺序），上次你选择了constant，这次就还是默认选中cosntant
  "editor.suggestSelection": "recentlyUsedByPrefix",
  // 编辑器：保存即格式化
  "editor.formatOnSave": true,
  // 编辑器：设置默认格式化工具 有了这个就不用每个类型的文件都设置一个格式化工具，大部分用prettier，只有特殊情况需要单独设置
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  // 编辑器：设置编辑器字体16号
  "editor.fontSize": 16,
  // 编辑器：保存时候的动作
  "editor.codeActionsOnSave": {
    // 保存的时候执行一次eslint校验
    "source.fixAll.eslint": true,
    // 保存的时候调整import顺序，按引入包字母顺序
    "source.organizeImports": true
  },
```

把这两个文件放到项目根目录，同时把setting.json的配置放到编辑器里边，入口可以参考下图
![image.png](https://blogimage.houjiyi.com/Fvz_n3_tDUbbbJocKvviGvfVRDVa)

然后是我本地的完整的配置，仅供参考：
```json
{
  // 文件：文件自动保存，只要暂停输入
  "files.autoSave": "afterDelay",
  // 文件：文件保存机制：暂停输入3000毫秒即保存
  "files.autoSaveDelay": 3000,
  // 编辑器：这个属性是用来设置建议值的，就是我们在输入co时，编辑器提示我们const这个功能。推荐设置为first——即每次默认选中推荐值第一个，比如你输入co，推荐值为const和constant（按顺序），那么就会选择const。
  // 当然，也推荐设置为recentlyUsedByPrefix，即上次你选择或者输入过什么，这次就默认选中什么，比如你输入co，推荐值为const和constant（按顺序），上次你选择了constant，这次就还是默认选中cosntant
  "editor.suggestSelection": "recentlyUsedByPrefix",
  // 编辑器：保存即格式化
  "editor.formatOnSave": true,
  // 编辑器：设置默认格式化工具 有了这个就不用每个类型的文件都设置一个格式化工具，大部分用prettier，只有特殊情况需要单独设置
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  // 编辑器：设置编辑器字体16号
  "editor.fontSize": 16,
  // 编辑器：保存时候的动作
  "editor.codeActionsOnSave": {
    // 保存的时候执行一次eslint校验
    "source.fixAll.eslint": true,
    // 保存的时候调整import顺序，按引入包字母顺序
    "source.organizeImports": true
  },
  // 编辑器：指定换行缩进空格数为2个空格
  "editor.tabSize": 2,
  "workbench.iconTheme": "vscode-icons",
  "workbench.startupEditor": "newUntitledFile",
  "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
  "i18n-ally.displayLanguage": "en",
  "debug.console.fontSize": 16,
  "terminal.integrated.fontSize": 16,
  "explorer.confirmDragAndDrop": false,
  "explorer.confirmDelete": false,
  "vsicons.dontShowNewVersionMessage": true,

  // vetue的一些配置
  "vetur.useWorkspaceDependencies": true,
  "vetur.validation.template": false,
  // 忽略项目警告，比如：vetur can't find 'tsconfig.json' or 'jsconfig.json'
  "vetur.ignoreProjectWarning": true,

  "prettier.tabWidth": 2,
  "prettier.singleQuote": true,
  // 指定每行代码的最佳长度， 如果超出长度则换行。
  // 有了prettier.config.js文件则应用文件配置
  "prettier.printWidth": 200,
  "prettier.useEditorConfig": false,
  "prettier.semi": false,
  "prettier.trailingComma": "none",

  "eslint.alwaysShowStatus": true
}
```

有了这些配置还不够，还需要安装两个依赖，否则你在使用eslint格式化的时候，会报错：
![image.png](https://blogimage.houjiyi.com/FlGgAHGSzXJGozXCnK48wP4QMJ6o)

```bash
"@babel/eslint-parser": "^7.12.16",
"@vue/eslint-config-standard": "^6.1.0",
```

安装完成以后再使用eslint的格式化可以将一些不规范的地方规范，同时与自动保存使用的prettire也不会冲突。

## 其他
格式化快捷键：ctrl+option+L
![image.png](https://blogimage.houjiyi.com/FmTFJJt0cZlPHVtQxPJ7IEapMgx4)

eslint格式化快捷键：ctrl+option+;
![image.png](https://blogimage.houjiyi.com/Flsf2ZXVLpMpbKR9i2kKByLqOdxG)

我本地的环境：
操作系统：MacBook Pro (15-inch, 2017)
nodejs版本：v16.9.0
npm版本：7.21.1
yarn版本：1.22.17
vue-cli版本：@vue/cli 5.0.0-rc.1


当然，我们在项目中配置了保存即格式化，实际上也能解决大部分问题，当遇到两种格式化不一样的时候，我们可以使用prettier来适配eslint，因为eslint对应的vue项目插件格式化还是比较通用的。

## 参考
> 关于eslint的配置及其说明可以看[腾讯文档](https://cloud.tencent.com/developer/section/1135587)

> eslint [中文官方文档](https://cn.eslint.org/docs/rules/)

> 关于prettier的配置及其说明可以看[官方文档](https://www.prettier.cn/docs/options.html)

> [eslint-plugin-vue](https://eslint.vuejs.org/rules/max-attributes-per-line.html) 这里是一些关于vue的格式化配置，可以通过这个文档了解vue格式化的思路