---
title: vscode上关于stylelint与prettier的格式化风格统一
tags:
  - Web开发
  - javascript
originContent: ''
categories:
  - Web开发
toc: false
date: 2022-04-02 11:24:36
---

> 我们开发的时候会用到eslint与prettier，同样在样式层面，也需要一个规范，这时候我们就需要用到stylelint，可以帮我们写出更好更规范的css样式。

## [](https://www.houjiyi.com/#%E5%88%9B%E5%BB%BA%E9%A1%B9%E7%9B%AE "创建项目")创建项目

`vue create demo`

![image.png](https://blogimage.houjiyi.com/FshPYw0RSWLxENmTBzrcNAdXkMac)

这里使用的是vue3.x+sass创建的项目

## [](https://www.houjiyi.com/#%E6%B8%85%E7%82%B9%E9%85%8D%E7%BD%AE "清点配置")清点配置

### [](https://www.houjiyi.com/#%E9%9C%80%E8%A6%81%E5%AE%89%E8%A3%85%E7%9A%84%E4%BE%9D%E8%B5%96 "需要安装的依赖")需要安装的依赖

将下边这些依赖放在package.json的devDependencies里边，然后在执行一遍`yarn install`把依赖给装上。

```json
"prettier": "^2.2.1",  
"stylelint": "^13.12.0",  
"stylelint-config-prettier": "^9.0.3",  
"stylelint-config-rational-order": "^0.1.2",  
"stylelint-config-standard": "^22.0.0",  
```

### [](https://www.houjiyi.com/#vscode%E9%9C%80%E8%A6%81%E5%AE%89%E8%A3%85%E7%9A%84%E6%8F%92%E4%BB%B6 "vscode需要安装的插件")vscode需要安装的插件

我们需要给vscode安装一个stylelint-plus插件，之所以装这个插件是因为它支持保存即格式化的配置。

![image.png](https://blogimage.houjiyi.com/FsBMH7bTGhhykichM4KVosFF_vQb)

### [](https://www.houjiyi.com/#%E8%B0%83%E6%95%B4%E9%A1%B9%E7%9B%AEsettings-json%E9%85%8D%E7%BD%AE "调整项目settings.json配置")调整项目settings.json配置

在项目根目录下增加`.vscode/settings.json`配置文件，它编辑器针对项目的配置；  
你可以根据自己的习惯进行修改，比较重要的就是保存格式化，保存后应用的格式化里有stylelint；

```json
{  
 // 文件：文件自动保存，只要暂停输入  
 "files.autoSave": "afterDelay",  
 // 文件：文件保存机制：暂停输入3000毫秒即保存  
 "files.autoSaveDelay": 3000,  
 // stylelint保存即格式化  
 "stylelint.autoFixOnSave": true,  
 "prettier.endOfLine": "auto"  
}  
```

### [](https://www.houjiyi.com/#%E5%A2%9E%E5%8A%A0stylelint%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6 "增加stylelint配置文件")增加stylelint配置文件

在项目根目录下放置文件`stylelint.config.js`

```javascript
module.exports = {  
 root: true,  
 extends: [  
 "stylelint-config-standard",  
 "stylelint-config-rational-order",  
 "stylelint-config-prettier",  
 ],  
 // https://stylelint.docschina.org/user-guide/rules/  
 rules: {  
 // 允许嵌套的深度最多 5 层  
 "max-nesting-depth": 5,  
 // 防止::deep报错  
 "selector-pseudo-element-no-unknown": null,  
 // 防止类似@mixin报错  
 "at-rule-no-unknown": null,  
 },  
};  
```
## [](https://www.houjiyi.com/#%E6%95%88%E6%9E%9C "效果")效果

完成上边的步骤以后，代码中不符合规范的地方会报错，保存文件后会格式化

![image.png](https://blogimage.houjiyi.com/Fpe9XnrrXEfCaAvsihU-MxZM8nLs)

保存后变为：

![image.png](https://blogimage.houjiyi.com/FrSB-M9bNlM7hR93rFiRIk_ohAZ_)

这里是调整了css属性的先后顺序

## [](https://www.houjiyi.com/#%E6%80%BB%E7%BB%93 "总结")总结

该配置在不同的项目上做过实验，比如less、sass,都是支持的，我们使用的是推荐配置，基本是能满足我们的需要的，我们的目的就是统一格式化样式，通过工具规范不要写出糟糕的代码，这样目的就达到了。如果在使用中发现规则不满足团队需要，改写stylelint.config.js的rules即可。
