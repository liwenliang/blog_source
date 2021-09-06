---
title: vue单页面项目返回上一页无效，链接变化了，但是页面没有变化
tags:
  - 知识点备忘
  - Web开发
  - 微前端
categories:
  - Web开发
toc: false
date: 2021-06-19 22:55:40
---

在最近的项目中，返回上一页没有效果，经过好久的排查才发现问题，是路由守卫写法不规范导致。

 

在项目中用路由守卫做了登录拦截，没登录的跳转到登录页面。页面跳转和拦截都没问题，但是返回上一页就不行了，也没有报错。

代码贴上来

```javascript
router.beforeEach((to, from, next) => {  
  if (to.meta.loginCheck) {     //登录判断，如果登录了则正常跳转，如没有登录，则先登录
    let gesToken =localStorage.getItem('gesToken');
    if(gesToken){ 
      next();
    } else{
      router.replace({path:'/login'});
　　　 next();　　//这行是重点，bug的原因就在这
}; } else { next(); }; });
```

本来以为next()的作用就是下一步跳转页面，后来发现，不能正常返回上一页，就是因为我使用完router.replace()之后，没有再使用next();

next()无论什么情况都是必须写的！！