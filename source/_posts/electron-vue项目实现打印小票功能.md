---
title: electron + vue项目实现打印小票功能
tags:
  - electron
originContent: >-
  前言：该文章需要一定的electron基础同学。如需了解更多相关信息，请移步electron官方文档。

  一、需求：

  公司项目需要通过electron调用系统打印机，实现打印小票的功能。


  二、分析：

  electron打印大概有两种：


  > 第一种：通过window的webcontent对象，使用此种方式需要单独开出一个打印的窗口，可以将该窗口隐藏，但是通信调用相对复杂。

  第二种：使用页面的webview元素调用打印，可以将webview隐藏在调用的页面中，通信方式比较简单。


  两个对象调用打印方法的使用方式都一样。


  本文是通过第二种方法实现静默打印。


  三、实现过程：

  1、要实现打印功能，首先要知道我们的设备上有哪些打印机。方法是：在渲染线程通过electron的ipcRenderer对象发送事件到主线程获取。（本文的渲染线程可以当做为一个print.vue文件）

  （1）主线程（electron.js）伪代码如下：

  ```javascript

  //引入electron

  import electron from 'electron';

  ```



  //创建一个浏览器对象

  ```javascript

  const window = new electron.BrowserWindow({
      width,
      height,
      frame: false,
      show: false,
      backgroundColor: '#4b5b79',
      minWidth: 1024,
      minHeight: 768,
      webPreferences: { webSecurity: false },
  });
    
  //在主线程下，通过ipcMain对象监听渲染线程传过来的getPrinterList事件

  electron.ipcMain.on('getPrinterList', (event) => {
      //主线程获取打印机列表
      const list = window.webContents.getPrinters();
      
      //通过webContents发送事件到渲染线程，同时将打印机列表也传过去
      window.webContents.send('getPrinterList', list);
  });

  ```


  （2）渲染线程(print.vue文件)伪代码如下：

  ```html

  <template>

  </template>

  <script>
      //引入ipcRenderer对象,该对象和主线程的ipcMain通讯
      import { ipcRenderer } from 'electron';
      
      //渲染线程主动发送getPrinterList事件到主线程请求打印机列表
      ipcRenderer.send('getPrinterList'); 
      
      //监听主线程获取到打印机列表后的回调
        ipcRenderer.once('getPrinterList', (event, data) => {
          //data就是打印机列表
          this.printList = data;
        });
  </script>


  //获取打印机列表完成

  ```


  2、（重头戏来了）获取打印机列表后，就需要通过electron自带的"webview"标签实现小票排版。"webview"是什么？可以把它当做"iframe"标签，它里面显示的是你需要打印的内容。

  （1）使用"webview"之前，需要新建一个print.html文件，把你要打印的内容通过print.html显示出来。我们项目的需求是将要打印的内容通过canvas画出后，再将canvas转成图片资源（base64），然后放到"webview"里面显示，伪代码如下：

  ```html

  <html lang="en">

  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
    //@page样式是用来设置打印机打印出来的样式，例如设置小票外边距样式
      @page {
        margin: 0px;
      }
    </style>
  </head>

  <body id='bd'>

  </body>

  <script>
      //引入ipcRenderer对象
    const {ipcRenderer} = require('electron')
    
    //监听渲染线程传过来的webview-print-render事件
    ipcRenderer.on('webview-print-render', (event, deviceInfo) => {
      // 动态创建一个img标签，然后插入到<body>中。deviceInfo是渲染线程传过来的数据
      let html = '';
      html = `<img src="${deviceInfo.imgSource}"
       width="${deviceInfo.imgWidth}px"
       height="${deviceInfo.imgHeight}px">`;
      document.getElementById('bd').innerHTML = html;
      
      //当图片插入到页面后，通过ipcRenderer对象的sendToHost方法和渲染线程通讯，告诉渲染线程打印的内容已经准备完毕，请开始打印操作
      ipcRenderer.sendToHost('webview-print-do');
    });
  </script>

  </html>


  ```


  复制代码

  （2）html文件创建完成后，将print.html引入到《webview
  src="./xxxx/print.html"》。该"webview"需要显式的定义在print.vue文件中，但需要将它用v-show="false"隐藏，不能用v-if，因为我们需要"webview"的dom节点存在于页面上，只是不展示而已。

  ```html

  <template>
    <div v-show="false">
      <webview ref="printWebview" src="./xxxx/print.html" nodeintegration/>
    </div>
  </template>


  <script>

  mounted() {
      //当vue节点渲染完成后，获取<webview>节点
      const webview = this.$refs.printWebview;
      
      //监听<webview>里面的消息，也就是监听print.html里面的ipcRenderer.sendToHost发送的事件，当该事件发送成功后就会进入下面的回调事件中执行打印操作。
      webview.addEventListener('ipc-message', (event) => {
        if (event.channel === 'webview-print-do') {
          //如果收到<webview>传过来的事件，名为"webview-print-do"，就执行 webview.print打印方法，打印<webview>里面的内容。
          webview.print(
            {
              //是否是静默打印
              silent: true,
              printBackground: true,
              //打印机的名称，就是本文一开始获得的打印机列表其中一个
              deviceName: 'xxx',
            },
            (data) => {
              //这个回调是打印后的回调事件，data为true就是打印成功，为false就打印失败
              console.log('webview success', data);
            },
          );
        }
      });
  },

  </script>

  ```


  到这里本electron调用打印机的功能就实现了。但本文章有很多细节没有讲到，只是大概的给了一个思路，如果写的有不对之处，还望见谅。


  作者：ding1379

  链接：https://juejin.im/post/5bf8b580e51d4522143b7b03

  来源：掘金

  著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
categories:
  - Web开发
toc: false
date: 2019-03-27 23:13:14
---

前言：该文章需要一定的electron基础同学。如需了解更多相关信息，请移步electron官方文档。
一、需求：
公司项目需要通过electron调用系统打印机，实现打印小票的功能。

<!-- more -->

二、分析：
electron打印大概有两种：

> 第一种：通过window的webcontent对象，使用此种方式需要单独开出一个打印的窗口，可以将该窗口隐藏，但是通信调用相对复杂。
第二种：使用页面的webview元素调用打印，可以将webview隐藏在调用的页面中，通信方式比较简单。

两个对象调用打印方法的使用方式都一样。

本文是通过第二种方法实现静默打印。

三、实现过程：
1、要实现打印功能，首先要知道我们的设备上有哪些打印机。方法是：在渲染线程通过electron的ipcRenderer对象发送事件到主线程获取。（本文的渲染线程可以当做为一个print.vue文件）
（1）主线程（electron.js）伪代码如下：
```javascript
//引入electron
import electron from 'electron';
```


//创建一个浏览器对象
```javascript
const window = new electron.BrowserWindow({
    width,
    height,
    frame: false,
    show: false,
    backgroundColor: '#4b5b79',
    minWidth: 1024,
    minHeight: 768,
    webPreferences: { webSecurity: false },
});
  
//在主线程下，通过ipcMain对象监听渲染线程传过来的getPrinterList事件
electron.ipcMain.on('getPrinterList', (event) => {
    //主线程获取打印机列表
    const list = window.webContents.getPrinters();
    
    //通过webContents发送事件到渲染线程，同时将打印机列表也传过去
    window.webContents.send('getPrinterList', list);
});
```

（2）渲染线程(print.vue文件)伪代码如下：
```html
<template>
</template>
<script>
    //引入ipcRenderer对象,该对象和主线程的ipcMain通讯
    import { ipcRenderer } from 'electron';
    
    //渲染线程主动发送getPrinterList事件到主线程请求打印机列表
    ipcRenderer.send('getPrinterList'); 
    
    //监听主线程获取到打印机列表后的回调
      ipcRenderer.once('getPrinterList', (event, data) => {
        //data就是打印机列表
        this.printList = data;
      });
</script>

//获取打印机列表完成
```

2、（重头戏来了）获取打印机列表后，就需要通过electron自带的"webview"标签实现小票排版。"webview"是什么？可以把它当做"iframe"标签，它里面显示的是你需要打印的内容。
（1）使用"webview"之前，需要新建一个print.html文件，把你要打印的内容通过print.html显示出来。我们项目的需求是将要打印的内容通过canvas画出后，再将canvas转成图片资源（base64），然后放到"webview"里面显示，伪代码如下：
```html
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
  //@page样式是用来设置打印机打印出来的样式，例如设置小票外边距样式
    @page {
      margin: 0px;
    }
  </style>
</head>
<body id='bd'>
</body>
<script>
    //引入ipcRenderer对象
  const {ipcRenderer} = require('electron')
  
  //监听渲染线程传过来的webview-print-render事件
  ipcRenderer.on('webview-print-render', (event, deviceInfo) => {
    // 动态创建一个img标签，然后插入到<body>中。deviceInfo是渲染线程传过来的数据
    let html = '';
    html = `<img src="${deviceInfo.imgSource}"
     width="${deviceInfo.imgWidth}px"
     height="${deviceInfo.imgHeight}px">`;
    document.getElementById('bd').innerHTML = html;
    
    //当图片插入到页面后，通过ipcRenderer对象的sendToHost方法和渲染线程通讯，告诉渲染线程打印的内容已经准备完毕，请开始打印操作
    ipcRenderer.sendToHost('webview-print-do');
  });
</script>
</html>

```

复制代码
（2）html文件创建完成后，将print.html引入到《webview src="./xxxx/print.html"》。该"webview"需要显式的定义在print.vue文件中，但需要将它用v-show="false"隐藏，不能用v-if，因为我们需要"webview"的dom节点存在于页面上，只是不展示而已。
```html
<template>
  <div v-show="false">
    <webview ref="printWebview" src="./xxxx/print.html" nodeintegration/>
  </div>
</template>

<script>
mounted() {
    //当vue节点渲染完成后，获取<webview>节点
    const webview = this.$refs.printWebview;
    
    //监听<webview>里面的消息，也就是监听print.html里面的ipcRenderer.sendToHost发送的事件，当该事件发送成功后就会进入下面的回调事件中执行打印操作。
    webview.addEventListener('ipc-message', (event) => {
      if (event.channel === 'webview-print-do') {
        //如果收到<webview>传过来的事件，名为"webview-print-do"，就执行 webview.print打印方法，打印<webview>里面的内容。
        webview.print(
          {
            //是否是静默打印
            silent: true,
            printBackground: true,
            //打印机的名称，就是本文一开始获得的打印机列表其中一个
            deviceName: 'xxx',
          },
          (data) => {
            //这个回调是打印后的回调事件，data为true就是打印成功，为false就打印失败
            console.log('webview success', data);
          },
        );
      }
    });
},
</script>
```

到这里本electron调用打印机的功能就实现了。但本文章有很多细节没有讲到，只是大概的给了一个思路，如果写的有不对之处，还望见谅。

作者：ding1379
链接：https://juejin.im/post/5bf8b580e51d4522143b7b03
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。