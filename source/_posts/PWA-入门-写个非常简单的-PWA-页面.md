---
title: 'PWA 入门: 写个非常简单的 PWA 页面'
tags:
  - PWA
  - 知识点备忘
originContent: ''
categories:
  - Web开发
toc: false
date: 2020-07-13 14:53:36
---

Progressive Web Apps 是 Google 提出的用前沿的 Web 技术为网页提供 App 般使用体验的一系列方案。这篇文章里我们来完成一个非常简单的 PWA 页面。

> 一个 PWA 应用首先是一个网页, 可以通过 Web 技术编写出一个网页应用. 随后添加上 App Manifest 和 Service Worker 来实现 PWA 的安装和离线等功能。下面的教程基于 [Migrate your site to a Progressive Web App](https://link.zhihu.com/?target=https%3A//codelabs.developers.google.com/codelabs/migrate-to-progressive-web-apps/index.html%3F%230) 和 Google 给出的 [sample](https://link.zhihu.com/?target=https%3A//github.com/GoogleChrome/samples/tree/gh-pages/service-worker/basic) 示例。完整代码访问 [minimal-pwa](https://link.zhihu.com/?target=https%3A//github.com/yong-chen04/minimal-pwa) 查看。


#### 准备工作
建议安装 [http-server](https://link.zhihu.com/?target=https%3A//www.npmjs.com/package/http-server) 和 [ngrok](https://link.zhihu.com/?target=https%3A//ngrok.com/) 以便调试和查看。


准备一个 HTML 文件, 以及相应的 CSS 等:

```html
<head>
  <title>Minimal PWA</title>
  <meta name="viewport" content="width=device-width, user-scalable=no" />
  <link rel="stylesheet" type="text/css" href="main.css">
</head>
<body>
  <h3>Revision 1</h3>
  <div class="main-text">Minimal PWA, open Console for more~~~</div>
</body>
```

#### 添加 manifest.json 文件

为了让 PWA 应用被添加到主屏幕, 使用 manifest.json 定义应用的名称, 图标等等信息。

```json
{
  "name": "Minimal app to try PWA",
  "short_name": "Minimal PWA",
  "display": "standalone",
  "start_url": "/",
  "theme_color": "#8888ff",
  "background_color": "#aaaaff",
  "icons": [
    {
      "src": "e.png",
      "sizes": "256x256",
      "type": "image/png"
    }
  ]
}
```

然后在 HTML 文件当中引入配置:

```html
<link rel="manifest" href="manifest.json" />
```

#### 添加 Service Worker

Service Worker 在网页已经关闭的情况下还可以运行, 用来实现页面的缓存和离线, 后台通知等等功能。sw.js 文件需要在 HTML 当中引入:

```html
<script>
  if (navigator.serviceWorker != null) {
    navigator.serviceWorker.register('sw.js')
    .then(function(registration) {
      console.log('Registered events at scope: ', registration.scope);
    });
  }
</script>
```

后面我们会往 sw.js 文件当中添加逻辑代码。在 Service Worker 当中会用到一些全局变量:


- self: 表示 Service Worker 作用域, 也是全局变量
- caches: 表示缓存
- skipWaiting: 表示强制当前处在 waiting 状态的脚本进入 activate 状态
- clients: 表示 Service Worker 接管的页面

#### 四级标题处理静态缓存

首先定义需要缓存的路径, 以及需要缓存的静态文件的列表, 这个列表也可以通过 Webpack 插件生成。
```javascript
var cacheStorageKey = 'minimal-pwa-1'

var cacheList = [
  '/',
  "index.html",
  "main.css",
  "e.png"
]
```

借助 Service Worker, 可以在注册完成安装 Service Worker 时, 抓取资源写入缓存:

```javascript
self.addEventListener('install', e => {
  e.waitUntil(
    caches.open(cacheStorageKey)
    .then(cache => cache.addAll(cacheList))
    .then(() => self.skipWaiting())
  )
})
```

调用 self.skipWaiting() 方法是为了在页面更新的过程当中, 新的 Service Worker 脚本能立即激活和生效。


#### 处理动态缓存

网页抓取资源的过程中, 在 Service Worker 可以捕获到 fetch 事件, 可以编写代码决定如何响应资源的请求:

```javascript
self.addEventListener('fetch', function(e) {
  e.respondWith(
    caches.match(e.request).then(function(response) {
      if (response != null) {
        return response
      }
      return fetch(e.request.url)
    })
  )
})
```

真实的项目当中, 可以根据资源的类型, 站点的特点, 可以专门设计复杂的策略。fetch 事件当中甚至可以手动生成 Response 返回给页面。

#### 更新静态资源

缓存的资源随着版本的更新会过期, 所以会根据缓存的字符串名称(这里变量为 cacheStorageKey, 值用了 "minimal-pwa-1")清除旧缓存, 可以遍历所有的缓存名称逐一判断决决定是否清除(备注: 简化的写法, Promise.all 中 return undefined 可能出错, 见评论):
```javascript
self.addEventListener('activate', function(e) {
  e.waitUntil(
    Promise.all(
      caches.keys().then(cacheNames => {
        return cacheNames.map(name => {
          if (name !== cacheStorageKey) {
            return caches.delete(name)
          }
        })
      })
    ).then(() => {
      return self.clients.claim()
    })
  )
})
```

在新安装的 Service Worker 中通过调用 self.clients.claim() 取得页面的控制权, 这样之后打开页面都会使用版本更新的缓存。旧的 Service Worker 脚本不再控制着页面之后会被停止。


#### 查看 Demo

执行命令:
```python
http-server -c-1 # 注意设置关闭缓存, 这里用参数 -c-1
# 用另一个终端
ngrok http 8080
```

桌面浏览器可以直接通过 http://localhost:8080 访问, 从 DevTools 的 Application 标签可以看到 Service Worker。


由于 Service Worker 限制了使用 HTTPS 地址或者 localhost 地址, 在 Android Chrome 打开需要借助 ngrok 生成的 HTTPS 地址, 这样才能把 demo 添加到首屏。添加到首屏之后, 即便在离线状态下, 页面也可以打开。

![image.png](http://blogimage.houjiyi.com/FpwQxFgVNH1q6GvPAwQym352GcuL)

从 DevTools 可以看到, 普通页面刷新时, 列表当中的静态资源都是从 Service Worker 获取的:

![image.png](http://blogimage.houjiyi.com/Ft4QfH0NL-81HC1jotmd6lqojFdy)

#### 更新页面
页面被缓存之后, 就需要适当处理缓存失效时页面的更新。在这个 Demo 当中, 被缓存的资源是无法发起请求判断是否被更新的, 只有 sw.js 会自动根据 HTTP 缓存的机制尝试去判断应用是否被更新。

所以当页面发生修改时, 要同时对 sw.js 文件进行一次修改。比如在 HTML 当中更新版本到 2:
```html
<h3>Revision 2</h3>
```

同时 sw.js 文件当中也要进行一次修改, 保证文件发生改变, 同时缓存的名称也变改变了:

```javascript
var cacheStorageKey = 'minimal-pwa-2'
```

然后重新打开一次页面, 这个时候渲染的页面依然是旧的, 不过可以从 DevTools 看到 sw.js 被安装和激活。之后关闭页面, 再次打开, 就可以见到网页上的显示版本变成了 2。



> 注意: Demo 当中如果直接启动 http-server 而不使用 -c-1 关闭缓存, sw.js 可能被缓存住, 导致更新方案失败。这种情况下存在 Caches API 和 HTML caching 两层缓存, 需要进行清理才能完成更新。

#### 更多
你还可以实现一个 App Shell, 可以用 Service Worker 实现后台通知等功能。


参考你的首个 [Progressive Web App](https://link.zhihu.com/?target=https%3A//developers.google.com/web/fundamentals/getting-started/codelabs/your-first-pwapp/%3Fhl%3Dzh-cn%23firebase) 了解更加详细的编写 PWA 应用的方式。