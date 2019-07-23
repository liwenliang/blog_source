---
title: CSS3实现波浪进度球
tags:
  - 知识点备忘
  - vue
originContent: |-
  ```html
  <template>
    <div class="fensug">
      <div class="waven">
        <p>100%</p>
        <div class="wave"></div>
      </div>
    </div>
  </template>

  <script>

    export default {
      name: 'Home',
      computed: {}
    }
  </script>

  <style scoped lang="scss">
    $size: 204px;
    .fensug {
      width: $size;
      height: $size;
      background: #fff;
      border: 4px #11D5C6 solid;
      margin: 100px auto;
      padding: 5px;
      border-radius: 50%;

      .waven {
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background: #11D5C6;
        overflow: hidden;
        position: relative;

        .wave {
          width: 408px;
          height: 20%;
          position: absolute;
          left: 0px;
          bottom: 0;
          background: url(../../assets/img/wave.png) repeat-x;
          animation: move_wave 1.8s linear infinite;
        }

        p {
          width: 100%;
          color: white;
          line-height: $size;
          font-weight: bold;
          font-size: 34px;
          text-align: center;
          z-index: 1;
          position: absolute;
        }
      }
    }

    @keyframes move_wave {
      0% {
        transform: translateX(0)
      }
      50% {
        transform: translateX(-25%)
      }
      100% {
        transform: translateX(-50%)
      }
    }
  </style>

  ```
  ![image.png](http://blogimage.houjiyi.com/FlMC8EJLTjDX7wAqwu-mkZXILxEw)
categories:
  - H5开发
  - Web开发
toc: false
date: 2019-07-23 19:34:45
---

想实现这样一个效果，于是网上搜集修改

![image.png](http://blogimage.houjiyi.com/Fl5ilJTLw1xcFDsiWrgiBk0SWS2l)

<!-- more -->

```html
<template>
  <div class="fensug">
    <div class="waven">
      <p>100%</p>
      <div class="wave"></div>
    </div>
  </div>
</template>

<script>

  export default {
    name: 'Home',
    computed: {}
  }
</script>

<style scoped lang="scss">
  $size: 204px;
  .fensug {
    width: $size;
    height: $size;
    background: #fff;
    border: 4px #11D5C6 solid;
    margin: 100px auto;
    padding: 5px;
    border-radius: 50%;

    .waven {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background: #11D5C6;
      overflow: hidden;
      position: relative;

      .wave {
        width: 408px;
        height: 20%;
        position: absolute;
        left: 0px;
        bottom: 0;
        background: url(../../assets/img/wave.png) repeat-x;
        animation: move_wave 1.8s linear infinite;
      }

      p {
        width: 100%;
        color: white;
        line-height: $size;
        font-weight: bold;
        font-size: 34px;
        text-align: center;
        z-index: 1;
        position: absolute;
      }
    }
  }

  @keyframes move_wave {
    0% {
      transform: translateX(0)
    }
    50% {
      transform: translateX(-25%)
    }
    100% {
      transform: translateX(-50%)
    }
  }
</style>

```
![image.png](http://blogimage.houjiyi.com/FlMC8EJLTjDX7wAqwu-mkZXILxEw)