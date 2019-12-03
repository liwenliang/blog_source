---
title: 浏览器播放RTSP直播流
tags:
  - Web开发
originContent: ''
categories:
  - 项目总结
toc: false
date: 2019-12-03 11:50:20
---

最近有个需求是将rtsp直播流在浏览器上直接播放，要求低延迟效果好，查并且尝试了一系列的方法，发现目前体验最好的是通过node将直播流转成二进制图片数据，然后在浏览器端绘制，用到的包有jsmpeg、node-rtsp-stream-jsmpeg

server端：服务器端需要安装ffmpeg，nodejs，如果是本地的话装在本地就可以了

```javascript
const Stream = require('node-rtsp-stream-jsmpeg')

const options = {
  name: 'streamName',
  url: 'rtsp://xxxx',
  wsPort: 3333
}

stream = new Stream(options)

stream.start()
```

client端：

```html
<!DOCTYPE html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>DEMO node-rtsp-stream-jsmpeg</title>
  <script src="jsmpeg.min.js"></script>
</head>
<body>
<div>
  <canvas id="video-canvas"></canvas>
</div>

<script type="text/javascript">
  var url = 'ws://xxxx:3333'
  var canvas = document.getElementById('video-canvas')
  var player = new JSMpeg.Player(url, { canvas: canvas, videoBufferSize: 1024*1024 })
</script>
</body>
```

整体体验效果不错。
