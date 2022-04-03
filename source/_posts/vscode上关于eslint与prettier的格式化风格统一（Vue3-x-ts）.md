---
title: vscode上关于eslint与prettier的格式化风格统一（Vue3.x+ts）
tags:
  - javascript
  - Web开发
originContent: ''
categories:
  - Web开发
toc: false
date: 2022-04-02 11:18:12
---

> 我们使用vue2.x+js的时候会用到eslint与prettier，同样在vue3.x里也会用到，只是我们观察业内主流项目发现，配置与2.x还是有些区别的，这里主要对vue3.x+ts的开发环境进行eslint+prettier的安装与使用方式进行说明

## [](https://www.houjiyi.com/#%E5%88%9B%E5%BB%BA%E6%96%B0%E7%9A%84vue3-x-ts%E9%A1%B9%E7%9B%AE "创建新的vue3.x+ts项目")创建新的vue3.x+ts项目

![image.png](https://blogimage.houjiyi.com/Fss2apJzpTUV65oiPKWVPhcYnrhE)

![image.png](https://blogimage.houjiyi.com/FgBkuyoL4F53FpdzcVHip91ZcEy8)

![image.png](https://blogimage.houjiyi.com/FkysG4EOBHIeSHuqVa1oDQ7HWsW5)

## [](https://www.houjiyi.com/#%E6%B8%85%E7%82%B9%E9%85%8D%E7%BD%AE "清点配置")清点配置

### [](https://www.houjiyi.com/#%E9%9C%80%E8%A6%81%E5%AE%89%E8%A3%85%E7%9A%84%E4%BE%9D%E8%B5%96 "需要安装的依赖")需要安装的依赖

安装需要的依赖  

```
yarn add -D eslint-plugin-vue@latest @typescript-eslint/eslint-plugin@latest @typescript-eslint/parser@latest eslint@^7.12.1 prettier@^2.2.1 @vue/cli-plugin-eslint@~4.5.0 @vue/eslint-config-prettier@^6.0.0 @vue/eslint-config-typescript@^7.0.0 eslint-plugin-prettier@^3.3.1  
```

```bash
"eslint-plugin-vue@latest"   
"@typescript-eslint/eslint-plugin@latest",  
"@typescript-eslint/parser@latest",  
"eslint@^7.12.1",  
"prettier@^2.2.1",  
"@vue/cli-plugin-eslint@~4.5.0",  
"@vue/eslint-config-prettier@^6.0.0",  
"@vue/eslint-config-typescript@^7.0.0",  
"eslint-plugin-prettier@^3.3.1",  
```

### [](https://www.houjiyi.com/#vscode%E9%9C%80%E8%A6%81%E5%AE%89%E8%A3%85%E7%9A%84%E6%8F%92%E4%BB%B6 "vscode需要安装的插件")vscode需要安装的插件

我们需要给vscode安装插件：”dbaeumer.vscode-eslint” “esbenp.prettier-vscode”

### [](https://www.houjiyi.com/#%E8%B0%83%E6%95%B4%E9%A1%B9%E7%9B%AEsettings-json%E9%85%8D%E7%BD%AE "调整项目settings.json配置")调整项目settings.json配置

在项目根目录下增加`.vscode/settings.json`配置文件，它编辑器针对项目的配置；

你可以根据自己的习惯进行修改，比较重要的就是保存格式化，保存后应用的格式化里有eslint；

``` javascript
{  
 // 文件：文件自动保存，只要暂停输入  
 "files.autoSave": "afterDelay",  
 // 文件：文件保存机制：暂停输入3000毫秒即保存  
 "files.autoSaveDelay": 3000,  
 // 编辑器：保存即格式化  
 "editor.formatOnSave": true,  
 // 关闭编辑器对 js 文件的格式化，交给 ESLint 来做格式化，否则会格式化两次  
 "\[javascript\]": {  
 "editor.formatOnSave": false  
 },  
 "prettier.endOfLine": "auto",  
 // 编辑器：设置默认格式化工具 有了这个就不用每个类型的文件都设置一个格式化工具，大部分用prettier，只有特殊情况需要单独设置  
 "editor.defaultFormatter": "esbenp.prettier-vscode",  
 // 编辑器：保存时候的动作  
 "editor.codeActionsOnSave": {  
 // 保存的时候执行一次eslint校验  
 "source.fixAll.eslint": true,  
 // 保存的时候调整import顺序，按引入包字母顺序  
 "source.organizeImports": true  
 }  
}  
```
### [](https://www.houjiyi.com/#%E6%B7%BB%E5%8A%A0%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E5%88%B0%E9%A1%B9%E7%9B%AE%E6%A0%B9%E7%9B%AE%E5%BD%95 "添加配置文件到项目根目录")添加配置文件到项目根目录

`.eslintrc.js` 文件规定了eslint的规范  

``` javascript
module.exports = {  
 root: true,  
 env: {  
 node: true,  
 },  
 extends: [  
 "plugin:vue/vue3-essential",  
 "eslint:recommended",  
 "@vue/typescript/recommended",  
 "@vue/prettier",  
 "@vue/prettier/@typescript-eslint",  
 ],  
 parserOptions: {  
 ecmaVersion: 2020,  
 },  
 rules: {  
 "no-console": process.env.NODE_ENV === "production" ? "warn" : "off",  
 "no-debugger": process.env.NODE_ENV === "production" ? "warn" : "off",  
 "vue/multi-word-component-names": 0,  
 },  
};  
```

`prettier.config.js`与`.prettierrc.js`文件规定了prettier的规范，增加endOfLine是为了兼容windows环境

```javascript
module.exports = {  
 endOfLine: "auto", // 结尾是 \\n \\r \\n\\r auto  
};  
```


### [](https://www.houjiyi.com/#%E5%AE%8C%E6%88%90%E6%95%88%E6%9E%9C "完成效果")完成效果

安装完以后会发现项目中一些文件开始有警告了，这时候ctrl+s保存文件即可以将文件格式化成我们想要的样子；  
![image.png](https://blogimage.houjiyi.com/FjPo8u6-9zz2jKhKgIUkM7-wg7FW)

保存格式化后：  
![image.png](https://blogimage.houjiyi.com/Fn1IE_uM2WihVWkC-ocdqTqphivk)

### [](https://www.houjiyi.com/#%E6%80%BB%E7%BB%93 "总结")总结

vue3.x+ts与2.x的区别主要在于单双引号，以及对象最后一个属性后的逗号，还有单行代码后的分号；我们尽量使用官方推荐配置，因此你看到eslint会比2.x更简单，实际上使用的是默认配置；在项目中如果有哪些地方不符合团队习惯，可以单独在eslint配置文件中增加或修改。
