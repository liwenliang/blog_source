---
title: webpack之proxyTable设置跨域
tags:
  - 知识点备忘
originContent: >-
  ## 为什么要使用proxyTable

  - **很简单，两个字，跨域。**

  -
  在平时项目的开发环境中，经常会遇到跨域的问题，尤其是使用vue-cli这种脚手架工具开发时，由于项目本身启动本地服务是需要占用一个端口的，所以必然会产生跨域的问题。当然跨域有多种解决方式，这里就不一一例举，下次弄篇文章单独讲，在使用webpack做构建工具的项目中使用proxyTable代理实现跨域是一种比较方便的选择。


  ## 如何使用proxyTable

  还是拿之前使用过的vue-cli举例。我们首先要在项目目录中找到根目录下config文件夹下的index.js文件。由于我们是在开发环境下使用，自然而然是要配置在dev里面：


  ```javascript

  dev: {
    env: require('./dev.env'),
    port: 8080,
    autoOpenBrowser: true,
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    proxyTable: {
      '/api': {
        target: 'http://www.abc.com',  //目标接口域名
        changeOrigin: true,  //是否跨域
        pathRewrite: {
          '^/api': '/api'   //重写接口
        }
      },
    cssSourceMap: false
  }

  ```

  上面这段代码的效果就是将本地8080端口的一个请求代理到了http://www.abc.com这一域名下：


  `'http://localhost:8080/api' ===> 'http://www.abc.com/api'`


  ## 没有统一项目名下的使用

  上面那种情况是有一个统一的项目名api的，所以说是比较好匹配的，就相当于说直接将以api开头的接口名代理一下换成目标域名访问就好了，可是如果说后台返给我们前端的接口没有了统一的项目名呢？之前，我是一个个单独去做了转换，接口少还没什么关系，但多了肯定是不现实的，前段时间在一次面试中受到了面试官的启发想到了这样一种取巧的方案：


  ```javascript

  //先人为给接口地址前面加上一个自定义的项目名

  let someApi = 'api' + '/xx/xx';


  dev: {
    env: require('./dev.env'),
    port: 8080,
    autoOpenBrowser: true,
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    proxyTable: {
      '/api': {
        target: 'http://www.abc.com',  //目标接口域名
        changeOrigin: true,  //是否跨域
        pathRewrite: {
          '^/api': '/'   //重写接口
        }
      },
    cssSourceMap: false
  }

  ```

  这里的项目名api是我们人为加上去的，经过代理之后就没了，这样我们在配置代理这里还是只需要配置一份就够了，只是在写接口地址时要注意区分开发环境和线上环境就可以了。


  ## 关于proxyTable的原理


  我在网上查了一下，这个代理实际上是利用http-proxy-middleware这个插件完成的，具体到这个插件的运行机制，由于是英文再加上能力有限就没深究了。但我想探究的是这种代理方式实际上是如何做到的，我看网上有人说实际上就是我们的本地服务器将请求转发给了目标服务器。之所以出现跨域是因为浏览器有同源策略的限制，但服务器是没有的，所以这种代理方式能够实现的机制大体就是：


  > 本地服务器 --》 代理 --》目标服务器 --》拿到数据后通过代理伪装成本地服务请求的返回值 ---》然后浏览器就顺利收到了我们想要的数据


  这是我的简单理解，按这个理解来说的话只要服务器允许跨域，任何人都能够拿到它的数据吗？那样同源策略不就大大弱化了吗？目前对这个问题还不是太理解，希望有想法的小伙伴留言指正！


  原文地址：[https://www.cnblogs.com/wancheng7/p/8987694.html](https://www.cnblogs.com/wancheng7/p/8987694.html)
categories:
  - Web开发
toc: false
date: 2019-07-01 15:23:46
---

这里我简单说明下自己需要记住的地方就是怎么实现跨域，当我们在开发的时候使用的是第三方的api，如果这个api不支持跨域，那么我们就要做一层代理，这个代理工作通常是由后端人员来完成的，原理也很简单就是搭建一个服务器，提供一个接口透传的机制，将跨域数据请求过来以后在传给自己的接口返回，换句话说跨域是浏览器里出现的，服务端是没有跨域这一说的，只要你有服务器就可以解决这个问题。当然在实际开发中我们后端可能还没搭建这个服务，那么前端在开发的时候完全可以使用proxyTable来实现自己跨域拿数据的目的。最后上线还是要接口支持跨域的，因为这时候没有本地开发环境也就没法实现跨域了。

详细内容如下：

<!-- more -->

## 为什么要使用proxyTable
- **很简单，两个字，跨域。**
- 在平时项目的开发环境中，经常会遇到跨域的问题，尤其是使用vue-cli这种脚手架工具开发时，由于项目本身启动本地服务是需要占用一个端口的，所以必然会产生跨域的问题。当然跨域有多种解决方式，这里就不一一例举，下次弄篇文章单独讲，在使用webpack做构建工具的项目中使用proxyTable代理实现跨域是一种比较方便的选择。

## 如何使用proxyTable
还是拿之前使用过的vue-cli举例。我们首先要在项目目录中找到根目录下config文件夹下的index.js文件。由于我们是在开发环境下使用，自然而然是要配置在dev里面：

```javascript
dev: {
  env: require('./dev.env'),
  port: 8080,
  autoOpenBrowser: true,
  assetsSubDirectory: 'static',
  assetsPublicPath: '/',
  proxyTable: {
    '/api': {
      target: 'http://www.abc.com',  //目标接口域名
      changeOrigin: true,  //是否跨域
      pathRewrite: {
        '^/api': '/api'   //重写接口
      }
    },
  cssSourceMap: false
}
```
上面这段代码的效果就是将本地8080端口的一个请求代理到了http://www.abc.com这一域名下：

`'http://localhost:8080/api' ===> 'http://www.abc.com/api'`

## 没有统一项目名下的使用
上面那种情况是有一个统一的项目名api的，所以说是比较好匹配的，就相当于说直接将以api开头的接口名代理一下换成目标域名访问就好了，可是如果说后台返给我们前端的接口没有了统一的项目名呢？之前，我是一个个单独去做了转换，接口少还没什么关系，但多了肯定是不现实的，前段时间在一次面试中受到了面试官的启发想到了这样一种取巧的方案：

```javascript
//先人为给接口地址前面加上一个自定义的项目名
let someApi = 'api' + '/xx/xx';

dev: {
  env: require('./dev.env'),
  port: 8080,
  autoOpenBrowser: true,
  assetsSubDirectory: 'static',
  assetsPublicPath: '/',
  proxyTable: {
    '/api': {
      target: 'http://www.abc.com',  //目标接口域名
      changeOrigin: true,  //是否跨域
      pathRewrite: {
        '^/api': '/'   //重写接口
      }
    },
  cssSourceMap: false
}
```
这里的项目名api是我们人为加上去的，经过代理之后就没了，这样我们在配置代理这里还是只需要配置一份就够了，只是在写接口地址时要注意区分开发环境和线上环境就可以了。

## 关于proxyTable的原理

我在网上查了一下，这个代理实际上是利用http-proxy-middleware这个插件完成的，具体到这个插件的运行机制，由于是英文再加上能力有限就没深究了。但我想探究的是这种代理方式实际上是如何做到的，我看网上有人说实际上就是我们的本地服务器将请求转发给了目标服务器。之所以出现跨域是因为浏览器有同源策略的限制，但服务器是没有的，所以这种代理方式能够实现的机制大体就是：

> 本地服务器 --》 代理 --》目标服务器 --》拿到数据后通过代理伪装成本地服务请求的返回值 ---》然后浏览器就顺利收到了我们想要的数据

这是我的简单理解，按这个理解来说的话只要服务器允许跨域，任何人都能够拿到它的数据吗？那样同源策略不就大大弱化了吗？目前对这个问题还不是太理解，希望有想法的小伙伴留言指正！

原文地址：[https://www.cnblogs.com/wancheng7/p/8987694.html](https://www.cnblogs.com/wancheng7/p/8987694.html)