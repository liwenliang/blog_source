---
title: JavaScript判断知否支持h5 video标签
date: 2017-10-27 14:36:10
tags: [javascript, h5, video]
---

也就一行代码：

```
!!document.createElement('video').canPlayType
```

记下来做个备忘