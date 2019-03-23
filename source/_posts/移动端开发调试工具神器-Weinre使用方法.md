---
title: 移动端开发调试工具神器--Weinre使用方法
tags:
  - 开发调试
originContent: >-
  前端开发调试必备：


  1. DOM操作断点调试；

  2. debugger断点调试；

  3. native方法hook（个人暂时还没有试过，不知效果如何）；

  4. 远程映射本地测试；

  5. Weinre移动调试（详细介绍）；


  像Dom断点调试和debugger断点调试我认为是大家经常用到的方法，或者是当前比较火狐下比较流行的Fiexbug调试工具；今天我们主要是研究Weinre调试工具的；


  当然，作为前端开发人员，令人比较乏味的即使手机端各个版本的支持程度，例如就拿iphone来说，虽然都是-webkit内核，如果你添加的动画，如-webkit-translate.......当然考虑到兼容性问题，你会带上前缀-webkit，但你本想手机端大多是支持HTML5和css3的，所以就试下了不带前缀-webkit，结果，呵呵，那么问题来了，iphone5s以下会有问题，以上就没有问题，所以你懂得，最好加上前缀-webkit，不过像最近比较新的版本的Andirod对CSS3的属性支持度还是不错的；


  接下来，我们介绍下重点：


  安装weinre


  weinre可以通过npm按照（个人也是比较推荐的）


  ```

  npm install -g weinre  (按照完成后，可以在cmd上，查看下版本号，看是否按照成功)

  ```


  按照之后，可以执行下面的命令来启动：


  ```

  weinre --httpPort 8080 --boundHost -all-

  ```


  ok!如果没有什么问题的话，我们打开谷歌浏览器（-webkit内核）输入：http://127.0.0.1:8080/   会看到以下界面


  以上这个便是庐山正面目了，上图的“debug client user
  interface”是weinre的Debug客户端，点击进入后看到目前还没有被测试的网页：


  Targets显示的none


  OK!那么我们继续，添加Debug Target


  有两种方式：


  1. Target Script（需要向页面中添加一个js）：


  ```

  <script src="http://192.168.1.69:8080/target/target-script-min.js#anonymous"
  type="text/javascript"></script>

  ```


  注意：标红的部分是你自己的IP地址和端口号，自己适配去调；


  2. 我们也可以将一段js保存到移动设备的书签中

  javascript:(function(e){e.setAttribute("src","http://127.0.0.1:8080/target/target-script-min.js#anonymous");document.getElementsByTagName("body")[0].appendChild(e);})(document.createElement("script"));void(0);



  最后：手机测试：

  1. 手机连接Wifi,必须和电脑在同一段网络，我用的是XAMPP模拟服务器，大家可以上网查用法，然后用自己的手机测试你要测试的网页即可；

  现在回到电脑端  http://127.0.0.1:8080    点击“debug client user interface:” 
  如果没有问题的话，就已经成功添加了Debug Target:


  提示：注意手机不要锁屏，不然调试会断开！


  2. 调试开始：


  自己可以查查各个组件！
categories:
  - H5开发
toc: false
date: 2018-04-17 14:06:56
---

前端开发调试必备：

1. DOM操作断点调试；
2. debugger断点调试；
3. native方法hook（个人暂时还没有试过，不知效果如何）；
4. 远程映射本地测试；
5. Weinre移动调试（详细介绍）；

像Dom断点调试和debugger断点调试我认为是大家经常用到的方法，或者是当前比较火狐下比较流行的Fiexbug调试工具；今天我们主要是研究Weinre调试工具的；

当然，作为前端开发人员，令人比较乏味的即使手机端各个版本的支持程度，例如就拿iphone来说，虽然都是-webkit内核，如果你添加的动画，如-webkit-translate.......当然考虑到兼容性问题，你会带上前缀-webkit，但你本想手机端大多是支持HTML5和css3的，所以就试下了不带前缀-webkit，结果，呵呵，那么问题来了，iphone5s以下会有问题，以上就没有问题，所以你懂得，最好加上前缀-webkit，不过像最近比较新的版本的Andirod对CSS3的属性支持度还是不错的；

接下来，我们介绍下重点：

安装weinre

weinre可以通过npm按照（个人也是比较推荐的）

```
npm install -g weinre  (按照完成后，可以在cmd上，查看下版本号，看是否按照成功)
```

按照之后，可以执行下面的命令来启动：

```
weinre --httpPort 8080 --boundHost -all-
```

ok!如果没有什么问题的话，我们打开谷歌浏览器（-webkit内核）输入：http://127.0.0.1:8080/   会看到以下界面

以上这个便是庐山正面目了，上图的“debug client user interface”是weinre的Debug客户端，点击进入后看到目前还没有被测试的网页：

Targets显示的none

OK!那么我们继续，添加Debug Target

有两种方式：

1. Target Script（需要向页面中添加一个js）：

```
<script src="http://192.168.1.69:8080/target/target-script-min.js#anonymous" type="text/javascript"></script>
```

注意：标红的部分是你自己的IP地址和端口号，自己适配去调；

2. 我们也可以将一段js保存到移动设备的书签中
javascript:(function(e){e.setAttribute("src","http://127.0.0.1:8080/target/target-script-min.js#anonymous");document.getElementsByTagName("body")[0].appendChild(e);})(document.createElement("script"));void(0);


最后：手机测试：
1. 手机连接Wifi,必须和电脑在同一段网络，我用的是XAMPP模拟服务器，大家可以上网查用法，然后用自己的手机测试你要测试的网页即可；
现在回到电脑端  http://127.0.0.1:8080    点击“debug client user interface:”  如果没有问题的话，就已经成功添加了Debug Target:

提示：注意手机不要锁屏，不然调试会断开！

2. 调试开始：

自己可以查查各个组件！