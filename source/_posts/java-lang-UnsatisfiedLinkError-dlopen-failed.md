---
title: 'java.lang.UnsatisfiedLinkError: dlopen failed'
tags:
  - apicloud
originContent: >-
  今天在做一个SDK开发的时候，因为library中用到了.so库文件，所以我直接从之前的项目中复制过来对应的.so文件到libs中，但是开发完成后，运行直接到native方法处就崩溃。。。我一脸蒙蔽哈。。。 

  这TM什么情况，在native处崩溃后，没有任何错误提示，搞的我1个小时定位问题。。。 

  最后实在搞不了了，还是写了个日志输出代码，发现这个错误：


  ```java.lang.UnsatisfiedLinkError: dlopen failed: file offset for the library
  "/data/app/com.shangxiaom.micms_library-1/lib/arm/librkecg.so" >= file size: 0
  >= 0
      at java.lang.Runtime.loadLibrary(Runtime.java:372)
      at java.lang.System.loadLibrary(System.java:1076)
      at rkhy.com.ecg.common.algorithm.RKECG.<clinit>(RKECG.java:5)
      at rkhy.com.ecg.common.algorithm.RKECG.initAlgorithm(Native Method)
      at com.shangxiaom.micms_library.RKECGActivity$4$1.run(RKECGActivity.java:173)
      at java.lang.Thread.run(Thread.java:818)

  ```


  这什么情况！！！我的.so库的size怎么会是0呢？？ 之前的项目一直在用啊~~


  我赶快打开工程的目录，看到这个.so库竟然真的是0.。。。。这什么情况啊~~复制过来的怎么size怎么会是0 呢？？完全摸不着头脑！！！！


  好吧~~ 既然这样，先修改问题吧。。 重新拷贝一下.so库文件吧。。。


  仅以此文记录此坑。~

  --------------------- 

  作者：慵懒的树獭 

  来源：CSDN 

  原文：https://blog.csdn.net/shangming150/article/details/81120664 

  版权声明：本文为博主原创文章，转载请附上博文链接！
categories:
  - 移动开发
toc: false
date: 2019-04-03 23:16:03
---

今天在做一个SDK开发的时候，因为library中用到了.so库文件，所以我直接从之前的项目中复制过来对应的.so文件到libs中，但是开发完成后，运行直接到native方法处就崩溃。。。我一脸蒙蔽哈。。。 
这TM什么情况，在native处崩溃后，没有任何错误提示，搞的我1个小时定位问题。。。 
最后实在搞不了了，还是写了个日志输出代码，发现这个错误：
<!--more-->
```java.lang.UnsatisfiedLinkError: dlopen failed: file offset for the library "/data/app/com.shangxiaom.micms_library-1/lib/arm/librkecg.so" >= file size: 0 >= 0
    at java.lang.Runtime.loadLibrary(Runtime.java:372)
    at java.lang.System.loadLibrary(System.java:1076)
    at rkhy.com.ecg.common.algorithm.RKECG.<clinit>(RKECG.java:5)
    at rkhy.com.ecg.common.algorithm.RKECG.initAlgorithm(Native Method)
    at com.shangxiaom.micms_library.RKECGActivity$4$1.run(RKECGActivity.java:173)
    at java.lang.Thread.run(Thread.java:818)

```

这什么情况！！！我的.so库的size怎么会是0呢？？ 之前的项目一直在用啊~~

我赶快打开工程的目录，看到这个.so库竟然真的是0.。。。。这什么情况啊~~复制过来的怎么size怎么会是0 呢？？完全摸不着头脑！！！！

好吧~~ 既然这样，先修改问题吧。。 重新拷贝一下.so库文件吧。。。

仅以此文记录此坑。~
--------------------- 
作者：慵懒的树獭 
来源：CSDN 
原文：https://blog.csdn.net/shangming150/article/details/81120664 
版权声明：本文为博主原创文章，转载请附上博文链接！