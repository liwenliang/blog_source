---
title: FE微前端架构实施
tags:
  - Web开发
  - 微前端
categories:
  - Web开发
  - 性能优化
toc: false
date: 2021-03-12 20:20:10
---

# 锋物物管系统-子应用模板

### 名词解释：
> 主应用===基座应用；子应用===需要运行在主应用下的应用，同时支持独立运行
> 
### 希望达到的目的
> 1. 代码库更小，更内聚、可维护性更高
> 2. 松耦合、自治的团队可扩展性更好
> 3. 渐进地升级、更新甚至重写部分前端功能成为了可能
> 4. 可以独立开发部署
> 

### 特点：
> 1. 独立开发运行部署。
> 2. 子应用的路由配置对应基座应用的路由配置上，以此方式来复用头部和侧边菜单。
> 3. 状态之间的传递，原则上是通过基座应用来进行的。
> 
### 当你拿到这个子应用模板以后，需要对其进行基本信息的改造：
> 1. package.json 中的name需要修改
> 2. src/router/index.js 中 `base: window.__POWERED_BY_QIANKUN__ ? '/microapp' : '/',`
> 中的microapp需要修改，他表示在基座应用中的第一级子目录。
> 3. 同时'microapp'在主应用中也会有对应的地方：比如我们把它放在了AppList.js这个文件中，这里边是
> 对子应用的配置
>   ```
>   export const AppList = [
>   {
>     name: 'microapp',
>     entry: '//localhost:10001',
>     container: '#appContainer',
>     activeRule: '/microapp/'
>    }
>   ]
>   ```
### 子应用的整体改造原则：
> 1. 路由配置在主应用要有一套，同时操作权限的配置在主应用下生效，独立运行不
> 需要生效，因为我们的权限相关配置原则上只在主应用下使用，如果独立查看子
> 应用，正常情况有权限看即意味着有权限看全部页面，否则维护两套环境的资源
> 和操作权限维护成本会非常高，非必要不建议这么去做。
> 
> 2. 主应用路由配置事例：fpms_pro/src/config/modules/MicroAppTest.js
> 
> 3. 如果要查看demo，url增加参数`debug=1`即可
> 
> 4. 开发独立项目时，需复制`fpms_app_template`项目，然后需要做这么几步：
> 1. package.json中name重新命名，例如：`fpms_app1`
> 2. `/src/config/router.config.js`中的路由，根据业务情况进行修改
> 3. 修改项目运行端口10001到其他端口，例如：10002，只要保证主应用中已经加载的子应用没用过即可
>
> 4. 然后再说下主应用中AppList.js需要添加的配置：
>   ```
>   {
>      name: 'app1',
>      entry: '//localhost:10002',
>      container: '#appContainer',
>      activeRule: '/app1/'
>   }
>   ```
> 5. 虽然当前架构支持任意开发语言进行子应用的开发，但这里还是建议使用相同的技术栈，便于后期维护
> 6. 子应用在独立运行时会展示自己的头部和左侧菜单，运行在基座应用中头部和左侧菜单不展示
> 7. 状态的传递通过基座应用，通过对基座应用某个状态的监听，在子应用中处理对应的逻辑，例如：groupItem
> 相关的配置在`main.js`的mount函数下。实际情况并不建议应用之间进行过多的数据传递，如果确实非常多，理论上应该是同一个应用。
> 8. 关于代码复用，当我们有精力可以构建并维护自己的组件库的时候，可以使用组件库的方式来对公共组件进行复用
> 9. 关于什么情况下使用子应用的方式进行开发部署；由于拆分成子应用也同样会带来维护上的成本，因此目前的拆分原则是根据团队进行拆分，例如一个独立团队对相对独立的模块进行开发；当然如果后期已有模块需要重构，则也可以使用这种方式进行，因为这也是微前端架构的一个优势。
> 10. 子应用应该有自己的域名，因此逻辑上是可以独立运行上线使用的，如果没有域名，可以通过path在nginx上做相关配置。
> 

### 关于部署上线
> 我们目前支持两种部署方式
#### 独立域名部署
> 
> 首先，子应用有自己的独立域名，并且支持https，例如：
>   1. microapp.pms.gmtech.top
> 
> 独立域名这种部署方式有些必要条件：
> 1. 资源需要支持CORS跨域请求，配置nginx
> 2. 域名支持https访问。
> 
> 相关需要调整文件的地方：
> 1. 主应用里AppList.js中的entry的写法：
> ```
>  {
>    name: 'microapp',
>    entry: getAppLoadUri('microapp', 10001),
>    container: '#appContainer',
>    activeRule: '/microapp/'
>  }
> entry获取合适的域名地址，这里方法为：
>
> const env = process.env.NODE_ENV
> function getAppLoadUri (appName, port) {
>   const hostName = location.hostname
>   const protocol = location.protocol
>   if (env === 'local') {
>     return `http://localhost:${port}/${appName}/`
>   } else if (env === 'development' || env === 'test') {
>     return `/${appName}/`
>   } else {
>     return `${protocol}//${appName}.${hostName}/${appName}/`
>   }
> }
>
> ```
> 2. 子应用中vue.config.js中,线上访问需要将静态资源上传到CDN上
> ```javascript
> const publicPath = process.env.NODE_ENV === 'production' && process.env.VUE_APP_PREVIEW !== 'true' ? 'https://pms-static.gmtech.top/fe/src/fpms_microapp/dist/' : '/microapp/'
> ```
> 3. 子应用中router/index.js中
> ```javascript
> base: window.__POWERED_BY_QIANKUN__ ? '/microapp' : '/'
> ```
> 4. 注意这几个地方的写法，独立部署和通过path部署是不一样的。
> 5. nginx 配置与单页应用类似，但需要支持cors配置
> ```
> server {
>       listen       80;
>       server_name pms-microapp-dev.gmtech.top;
>       add_header Access-Control-Allow-Methods *;
>       add_header Access-Control-Allow-Origin $http_origin;
>       add_header Access-Control-Allow-Credentials true;
>       add_header Access-Control-Allow-Headers Token,groupid,app,appid,projectid,project_id,Project-Id,appcode,Content-Type,Upgrade,Connection,X-Real-IP,Company-Id,companyID;
>       add_header Access-Control-Max-Age 86400;
>       if ($request_method = OPTIONS){
>           return 200;
>       }
>   
>       location / {
>           root   /home/work/www/fe/fpms_microapp/dist;
>           proxy_set_header Host $host;
>           index  index.html;
>           try_files $uri $uri/ /index.html;
>       }
> }
> ```
> 
#### path区分子项目部署
> 如果每个项目都要有独立域名才能使用的话，目前的微前端架构实际上是有缺陷的，因为我可能是想把一个系统
> 的多个模块进行拆分，方便维护，因此，通过path来区分子项目也应该支持：由于这种方式使用的是相同的域名
> 实际操作起来会节省掉很多由于资源跨域导致的无法加载的问题。也更适合把一个独立的大项目拆分成小应用的
> 场景。
>
> 相关需要调整文件的地方：
> 1. 主应用里AppList.js中的entry的写法：
> ```
> entry: getAppLoadUri('microapp', 10001),
> ```
> 2. 子应用中vue.config.js中,线上访问需要将静态资源上传到CDN上
> ```javascript
> const publicPath = process.env.NODE_ENV === 'production' && process.env.VUE_APP_PREVIEW !== 'true' ? 'https://pms-static.gmtech.top/fe/src/fpms_microapp/dist/' : '/microapp/'
> ```
> 3. 子应用中router/index.js中base始终保持'/'会比较合适，因为子应用与子应用之间的跳转需要使用path方式，如果配置了base则跳转会出现问题。
> 4. 注意这几个地方的写法，path部署和通过域名部署是不一样的。
> 5. nginx配置方式
> ```
> server {
>     listen       80;
>     server_name pms-dev.gmtech.top;
> 
>     gzip on;               #开启gzip压缩功能
>     gzip_static on;        #这个地方很重要，配置了vue打包gz文件以后可以直接使用，无需nginx再执行打包动作，节省服务器资源
>     gzip_min_length 1k;    #设置允许压缩的页面最小字节数 vue打包配置需要生成.gz文件的地方也是1k
>     gzip_buffers 4 16k;    #设置压缩缓冲区大小，此处设置为4个16K内存作为压缩结果流缓存
>     gzip_http_version 1.0; #http协议版本 默认1.1,实际上我们用的1.0,如果不配置gzip不生效
>     gzip_comp_level 2;     #设置压缩比率，最小为1，处理速度快，传输速度慢；9为最大压缩比，处理速度慢，传输速度快
>     gzip_types text/css text/xml application/javascript; #制定压缩的类型
>     gzip_vary on;          #选择支持vary header；改选项可以让前端的缓存服务器缓存经过gzip压缩的页面
> 
>     location / {
>         root   /home/work/www/fe/fpms_pro/dist;
>         proxy_set_header Host $host;
>         index  index.html;
>         try_files $uri $uri/ /index.html;
>         location = /index.html {
>             add_header Cache-Control "no-cache, no-store";
>         }
>     }
> 
>     location /microapp{
>       alias /home/work/www/fe/fpms_microapp/dist;
>       index  index.html;
>       try_files $uri $uri/ /index.html;
>     }
> }
> ```
>
> 有了这些内容，我们想如何进行一个项目的开发都变得非常灵活

### 当前应用的部署方式
> 目前我们的线上环境使用的是独立域名部署方式，因为在线上是部署在K8S上，不同子应用需要能够独立运行，所以这里必须给每个子应用一个独立的域名。
> 但是为了兼顾便捷性，我们在开发环境和测试环境使用的是path方式，这样能够节省维护子域名的成本。
> 本地开发使用localhost，dev环境和test环境使用path配置，线上和预发布环境使用子域名。

### 参考
> 项目参考乾坤框架：https://qiankun.umijs.org/zh/guide
> 
> 在保证业务需求的前提下，使用渐进式的改造方案，避免因为改造造成项目的大量修改
>
> 微前端的前提，还是得有主体应用，然后才有微组件或微应用，解决的是可控体系下的前端协同开发问题（含空间分离带来的协作和时间延续带来的升级维护）