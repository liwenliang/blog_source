---
title: 使用js判断浏览器
date: 2018-06-08 20:54:00
tags: [javascript, ua]
---

直接看代码：

```
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
<script >
    var userAgent = navigator.userAgent;
    var isOpera = userAgent.indexOf("Opera") > -1;
    var isMaxthon = userAgent.indexOf("Maxthon") > -1 ;
    var isIE = userAgent.indexOf("compatible") > -1 && userAgent.indexOf("MSIE") > -1 && !isOpera ;
    var isFF = userAgent.indexOf("Firefox") > -1 ;
    var isSafari = userAgent.indexOf("Safari") > -1 && userAgent.indexOf("Chrome") < 1 ;
    var isChrome = userAgent.indexOf("Chrome") > -1 ;
    var isIE5 = isIE55 = isIE6 = isIE7 = isIE8 = false;

    if(isFF){document.write("当前浏览器是  Firefox");}
    if(isMaxthon){document.write("当前浏览器是 傲游(webkit核心)");}
    if(isOpera){document.write("当前浏览器是  Opera");}
    if(isSafari){document.write("当前浏览器是  Safari");}
    if(isChrome){document.write("当前浏览器是  Chrome");}
    if(isIE){document.write("当前浏览器是  IE");}
</script>
```
