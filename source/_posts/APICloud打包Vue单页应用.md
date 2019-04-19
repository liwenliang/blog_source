---
title: APICloud打包Vue单页应用
tags:
  - apicloud
  - vue
originContent: >-
  APICloud新建项目后，会生成以下目录结构


  ![image.png](http://pok5gaadc.bkt.clouddn.com/Fr2qGfMeERsyzXVXNvD2fKfBvoXV)


  其中index.html是入口文件，而vue-cli打包生成的文件是在dist目录下

  ```bash

  ├─dist

  │ └─static

  │ 　　 ├─css

  │ 　　 └─js

  │ └─index.html

  ```


  把dist目录中的static文件夹和index.html复制到apicloud的项目文件中即可，覆盖原来的index.html。最终apicloud的目录结构只是比原来多了一个static目录。

   

  vue项目中需要做如下配置，config/index.js中需要把build的assetsPublicPath属性由原来的 /改成 ./，


  index.html添加一些代码用于处理物理返回键：

  ```html

  <!--引用apicloud的脚本(可以不加)-->

  <script src="script/api.js"></script>


  <div id="app"></div>

  <!-- built files will be auto injected -->


  <script>
    //这里处理物理返回键，避免打包后点击一次直接退出应用
    window.apiready = function () {
      var timer = 0
      api.addEventListener({
        name: 'keyback'
      }, function (ret, err) {
        var currentTime = +new Date()
        api.historyBack(function (ret, err) {
          if (!ret.status) {
            if ((currentTime - timer) > 2000) {
              timer = currentTime
              api.toast({
                msg: '再按一次返回键退出app',
                duration: 2000,
                location: 'bottom'
              })
            } else {
              api.closeWidget({
                id: 'A6089289836112', //这里改成自己的应用ID
                retData: {name: 'closeWidget'},
                silent: true
              })
            }
          }
        })
      })
    }
  </script>


  ```


  这样打包以后代码还存在index.html里，引用脚本的路径也是apicloud目录下的。之后把dist目录下的文件复制过去就行了。如此一来就能畅享vue带来的高效开发了。

   

  最后，如果想调用apicloud的组件，可以在vue的组件中这样写：

  ```html

  　　<button @click="clickHandler">点击试试</button>

      mounted() {

      },
      methods: {
        clickHandler() {
          //调用apicloud的组件
          var photoSelect = api.require('photoSelect');
          photoSelect.openAblum({
            permitnum: '9'
          }, function (ret, err) {
            if (ret) {
              alert(JSON.stringify(ret));
            } else {
              alert(JSON.stringify(err));
            }
          });
        }
      },
      created() {

      }

  ```


  原文地址：https://www.cnblogs.com/undefined000/p/9559694.html
categories:
  - 移动开发
toc: false
date: 2019-03-30 14:05:29
---

APICloud新建项目后，会生成以下目录结构

![image.png](http://blogimage.houjiyi.com/Fr2qGfMeERsyzXVXNvD2fKfBvoXV)

其中index.html是入口文件，而vue-cli打包生成的文件是在dist目录下
```bash
├─dist
│ └─static
│ 　　 ├─css
│ 　　 └─js
│ └─index.html
```

把dist目录中的static文件夹和index.html复制到apicloud的项目文件中即可，覆盖原来的index.html。最终apicloud的目录结构只是比原来多了一个static目录。

 

vue项目中需要做如下配置，config/index.js中需要把build的assetsPublicPath属性由原来的 /改成 ./，

index.html添加一些代码用于处理物理返回键：
```html
<!--引用apicloud的脚本(可以不加)-->
<script src="script/api.js"></script>

<div id="app"></div>
<!-- built files will be auto injected -->

<script>
  //这里处理物理返回键，避免打包后点击一次直接退出应用
  window.apiready = function () {
    var timer = 0
    api.addEventListener({
      name: 'keyback'
    }, function (ret, err) {
      var currentTime = +new Date()
      api.historyBack(function (ret, err) {
        if (!ret.status) {
          if ((currentTime - timer) > 2000) {
            timer = currentTime
            api.toast({
              msg: '再按一次返回键退出app',
              duration: 2000,
              location: 'bottom'
            })
          } else {
            api.closeWidget({
              id: 'A6089289836112', //这里改成自己的应用ID
              retData: {name: 'closeWidget'},
              silent: true
            })
          }
        }
      })
    })
  }
</script>

```

这样打包以后代码还存在index.html里，引用脚本的路径也是apicloud目录下的。之后把dist目录下的文件复制过去就行了。如此一来就能畅享vue带来的高效开发了。

 

最后，如果想调用apicloud的组件，可以在vue的组件中这样写：
```html
　　<button @click="clickHandler">点击试试</button>

    mounted() {

    },
    methods: {
      clickHandler() {
        //调用apicloud的组件
        var photoSelect = api.require('photoSelect');
        photoSelect.openAblum({
          permitnum: '9'
        }, function (ret, err) {
          if (ret) {
            alert(JSON.stringify(ret));
          } else {
            alert(JSON.stringify(err));
          }
        });
      }
    },
    created() {

    }

```

原文地址：https://www.cnblogs.com/undefined000/p/9559694.html