---
title: web页面播放amr文件
tags:
  - 知识点备忘
categories:
  - Web开发
toc: false
date: 2019-04-18 15:26:17
---

> git地址：[https://github.com/alex374/amr-player](https://github.com/alex374/amr-player)

demo如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
  <script src="./amrnb.js"></script>
  <script src="./amrplayer.js"></script>
  <script type="text/javascript">
    var player = new AmrPlayer('http://yxl.github.io/opencore-amr-js/yuan.amr', function(){
      console.log('--------success--------')
      console.log(`success`)
      console.log('--------success--------')
    }, function(e){
      console.log('--------process--------')
      console.log(e)
      console.log('--------process--------')
    });
  </script>
</head>
<body>
<input id="button" type="button" value="button">
<script type="text/javascript">
  document.getElementById('button').addEventListener('click', function(){
    player.play()
  })
</script>
</body>
</html>
```
