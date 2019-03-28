---
title: vue条形码和二维码
tags:
  - 知识点备忘
originContent: "先上效果图：\n\n![image.png](http://pok5gaadc.bkt.clouddn.com/Fj469wO3rqk9MbPqM0lh4bVQusYO)\n<!--more-->\n### 条形码：\n1. 安装jsbarcode\n```bash \nnpm install\_jsbarcode --save\n```\n2. 在使用的页面：\n```javascript\nimport JsBarcode from 'jsbarcode'\n```\n\n3. html\n```html\n<img id=\"barcode\" />\n```\n4. js: \n```javascript\nmounted(){\_ \_ \_ \_ \_ \_ \n  JsBarcode(\"#barcode\", \"\", {\n\_   format: \"CODE128\",//选择要使用的条形码类型\_ \_ \_ \_ \_ \_ \_ \_ \n    text: '条形码的数据',\_ \_ \_ \_ \_ \_ \_ \_ \n    displayValue: true,//是否在条形码下方显示文字\_ \_ \_ \_ \_ \_ \_ \_ \n    textPosition: \"bottom\"//设置文本的垂直位置\_ \_ \_ \_ \_ \_ \n  })\_ \_  \_ \_ \_ \_ \_ \_ \n}\n```\n\n### 二维码：\n1. npm install\_jr-qrcode --save\n2. 在使用的页面：  import jrQrcode from \"jr-qrcode\"\n3. html：**qrcodeImg**在**data**里面声明一下\n```html\n<img :src=\"qrcodeImg\" alt=\"\" class=\"qrcodeImg\">\n```\n4. js:\n```javascript\nmounted() {\_ \_ \_ \_ \_ \_ \n  this.qrcodeImg = jrQrcode.getQrBase64('条形码的数据')\_ \_ \_ \_ \n}\n```\n\n作者：小丸子超级可爱\n链接：https://www.jianshu.com/p/7453e7247c3c\n来源：简书\n简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。"
categories:
  - Web开发
toc: false
date: 2019-03-28 17:43:44
---

先上效果图：

![image.png](http://pok5gaadc.bkt.clouddn.com/Fj469wO3rqk9MbPqM0lh4bVQusYO)
<!--more-->
### 条形码：
1. 安装jsbarcode
```bash 
npm install jsbarcode --save
```
2. 在使用的页面：
```javascript
import JsBarcode from 'jsbarcode'
```

3. html
```html
<img id="barcode" />
```
4. js: 
```javascript
mounted(){            
  JsBarcode("#barcode", "", {
    format: "CODE128",//选择要使用的条形码类型                
    text: '条形码的数据',                
    displayValue: true,//是否在条形码下方显示文字                
    textPosition: "bottom"//设置文本的垂直位置            
  })                 
}
```

### 二维码：
1. npm install jr-qrcode --save
2. 在使用的页面：  import jrQrcode from "jr-qrcode"
3. html：**qrcodeImg**在**data**里面声明一下
```html
<img :src="qrcodeImg" alt="" class="qrcodeImg">
```
4. js:
```javascript
mounted() {            
  this.qrcodeImg = jrQrcode.getQrBase64('条形码的数据')        
}
```

作者：小丸子超级可爱
链接：https://www.jianshu.com/p/7453e7247c3c
来源：简书
简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。