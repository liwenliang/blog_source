---
title: koajs框架下创建二维码生成服务
tags:
  - koajs
originContent: >-
  通过node-qrcode项目能够快速搭建一个生成二维码的服务器，但是在各种框架下如何使用呢，比如我们用的koajs，同时希望这是一个动态生成图片的服务，也就是返回的就是图片信息，而不是通过一次get请求得到图片url，然后再把url放到图片src中，这种方式一来会增加客户端的复杂程度，二来生成的图片需要服务器资源存储


  <!-- more -->


  这时候我们就需要一个类似服务的链接，将需要二维码展示的字符串编码后传给后端则能得到一个需要的图片，代码如下：


  ```


  /**
   * 获取二维码接口
   *
   * txt 参数为二维码展示内容，如果不存在或者为空默认生成乐视网地址
   *
   * 接口地址：http://myhost/service/getQrCode/encodeuricomponentStr
   *
   */

  var QRCode = require('qrcode');


  /**
   * 通过包装方法，能够使用yield同步获取得到结果
   * @param text
   * @returns {Function}
   */
  var getQRCode = function(text) {
      return function (done) {
          QRCode.toDataURL(text, {
              margin: 1
          }, function (err, dataUrl) {
              done(null, dataUrl);
          });
      }
  }


  /**
   *
   * 经过一番尝试这样处理方式比较合理，不用操作文件
   * 尝试1：QRCode.toFile生成图片存到本地，通过fs读文件，返回浏览器，可以使用但是占用磁盘资源，维护成本相对较大
   * 尝试2：QRCode.toDataURL得到图片的base64格式，删除掉前边的固定图片标识，将base64转成buffer通过koajs输出
   * 尝试2的做法比较优雅，不会产生文件，更不会存在文件读写操作，只要服务器是稳定的，图片就一定能够出来
   * 
   * 缺点就是当服务失效后，所有二维码将不再存在。
   * 
   * 由于我们的参数传递是放在路径里，当然也能放在查询字符串里，这里用的是路径
   * 好处就是能够使用cdn缓存，如果服务挂了，访问过的链接二维码图片在cdn还能缓存一段时间
   * 同时能减少一部分服务器压力
   */
  exports.init = function* () {
      var txt = decodeURIComponent(this.params.txt || 'http:myhost');
      var dataUrl = yield getQRCode(txt);
      // 去掉生成dataUrl里边的固定标识，剩下的就是base64字符
      var base64 = dataUrl.replace(/^data:image\/\w+;base64,/, "")
      var dataBuffer = new Buffer(base64, 'base64');
      this.set('content-type', 'image/png');
      this.status = 200;
      this.body = dataBuffer;
  };


  ```


  最后验证CDN是否生效的方法也很简单，监听特定请求的二维码请求，第一次能够打到服务器上，之后的请求不再走服务器即可

  ```
   tail -f /var/logs/nginx/access.log | grep "service/getQrCode/myuniquestr"
  ```
categories:
  - NodeJS
toc: false
date: 2017-11-01 17:50:10
---

通过node-qrcode项目能够快速搭建一个生成二维码的服务器，但是在各种框架下如何使用呢，比如我们用的koajs，同时希望这是一个动态生成图片的服务，也就是返回的就是图片信息，而不是通过一次get请求得到图片url，然后再把url放到图片src中，这种方式一来会增加客户端的复杂程度，二来生成的图片需要服务器资源存储

<!-- more -->

这时候我们就需要一个类似服务的链接，将需要二维码展示的字符串编码后传给后端则能得到一个需要的图片，代码如下：

```

/**
 * 获取二维码接口
 *
 * txt 参数为二维码展示内容，如果不存在或者为空默认生成乐视网地址
 *
 * 接口地址：http://myhost/service/getQrCode/encodeuricomponentStr
 *
 */

var QRCode = require('qrcode');

/**
 * 通过包装方法，能够使用yield同步获取得到结果
 * @param text
 * @returns {Function}
 */
var getQRCode = function(text) {
    return function (done) {
        QRCode.toDataURL(text, {
            margin: 1
        }, function (err, dataUrl) {
            done(null, dataUrl);
        });
    }
}

/**
 *
 * 经过一番尝试这样处理方式比较合理，不用操作文件
 * 尝试1：QRCode.toFile生成图片存到本地，通过fs读文件，返回浏览器，可以使用但是占用磁盘资源，维护成本相对较大
 * 尝试2：QRCode.toDataURL得到图片的base64格式，删除掉前边的固定图片标识，将base64转成buffer通过koajs输出
 * 尝试2的做法比较优雅，不会产生文件，更不会存在文件读写操作，只要服务器是稳定的，图片就一定能够出来
 * 
 * 缺点就是当服务失效后，所有二维码将不再存在。
 * 
 * 由于我们的参数传递是放在路径里，当然也能放在查询字符串里，这里用的是路径
 * 好处就是能够使用cdn缓存，如果服务挂了，访问过的链接二维码图片在cdn还能缓存一段时间
 * 同时能减少一部分服务器压力
 */
exports.init = function* () {
    var txt = decodeURIComponent(this.params.txt || 'http:myhost');
    var dataUrl = yield getQRCode(txt);
    // 去掉生成dataUrl里边的固定标识，剩下的就是base64字符
    var base64 = dataUrl.replace(/^data:image\/\w+;base64,/, "")
    var dataBuffer = new Buffer(base64, 'base64');
    this.set('content-type', 'image/png');
    this.status = 200;
    this.body = dataBuffer;
};

```

最后验证CDN是否生效的方法也很简单，监听特定请求的二维码请求，第一次能够打到服务器上，之后的请求不再走服务器即可
```
 tail -f /var/logs/nginx/access.log | grep "service/getQrCode/myuniquestr"
```