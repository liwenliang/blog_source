---
title: macOS 第三方输入法使用 caps_lock 切换中英文和大小写
tags:
  - 知识点备忘
originContent: ''
categories:
  - Web开发
toc: false
date: 2022-04-02 11:51:40
---

# macOS 第三方输入法使用 caps_lock 切换中英文和大小写

我使用的系统是 macOS Catalina，系统自带的输入法是使用 **caps\_lock** 单一按键来切换中英文和大小写的。前提是在系统偏好设置中勾选上“**使用中/英键切换“ABC”输入模式**“。

![image.png](https://blogimage.houjiyi.com/FvA3d7aNfdgj6bRg6UNgLGU7WKDw)

这种自带切换方法效果是这样：

1.  单击一下 **caps\_lock** 键则切换中英文输入法。
2.  长按 **caps\_lock** 键则触发大写锁定，同时亮起 **caps\_lock** 灯。
3.  当已经处于大写锁定状态下时，再次单击 **caps\_lock** 键则会退出大写锁定状态，同时灭掉 **caps\_lock** 灯。

我个人是比较喜欢自带的切换方法的，用 **caps\_lock** 键切换中英文要比用 **ctrl+空格键** 方便得多，而且还能通过长按的方式兼顾大小写的转换，用一个按键搞定所有输入法切换的问题，还是很优雅的。

唯一的问题就是：苹果自带的拼音输入法很不好用，而如果使用了第三方输入法的话（比如搜狗输入法），在输入法选项中就会失去 “**使用中/英键切换“ABC”输入模式**“ 这一个选项，而只能使用 “**ctrl+空格键**” 之类的快捷键来切换中英文。

![image.png](https://blogimage.houjiyi.com/FhNYsogdUiDgvMme5gXVoA4sdDQh)

好在我发现了一种方法能即使用第三方输入法，又能使用单一的 caps\_lock 键来切换中英文和大小写。

需要使用的软件是 **[karabiner elements](https://link.zhihu.com/?target=https%3A//pqrs.org/osx/karabiner/)**。

[karabiner elements​pqrs.org/osx/karabiner/](https://link.zhihu.com/?target=https%3A//pqrs.org/osx/karabiner/)

首先安装软件 **[karabiner elements](https://link.zhihu.com/?target=https%3A//pqrs.org/osx/karabiner/)**，此软件免费。

然后确认当前切换输入法的快捷键是 【control+空格】：

进入 系统偏好设置 -> 键盘 -> 快捷键，设置成如下：

![image.png](https://blogimage.houjiyi.com/Fqpo7IARbigAnZHTTYNqTXRxaxz_)

以上两步完成之后则需要导入规则文件，有两种方法：一种是自建规则文件。此外，为了方便大家使用，我已将写好规则文件上传到软件的官网上，因此还可使用网上导入的方法自动导入规则文件。下面分别介绍一下：

## 方法一：官网自动导入规则文件（推荐）

1.  访问： [https://ke-complex-modifications.pqrs.org/](https://link.zhihu.com/?target=https%3A//ke-complex-modifications.pqrs.org/%3Fq%3Dchinese)
2.  搜索 【Chinese】
3.  在结果中找到【Toggle Chinese English With caps\_lock】，点击后面的 【Import】

![image.png](https://blogimage.houjiyi.com/FvOAJfWLXotNgGbQZmWGieMYZhOK)

4\. 在弹出的对话框中完成导入，在软件界面点击列表中【Toggle Chinese English With caps\_lock】后面的 【Enable】即可。

![image.png](https://blogimage.houjiyi.com/FnmkGapSit_hJzV6vnGPZu5deSc2)

* * *

如果官方网站搜索不到或者打不开官方网站的话，就使用下面的自建规则文件方法：

## **方法二： 自建规则文件**

1.  **新建规则文件：**

新建一个文本文件，并命名为 **caps\_lock\_toggle\_chinese\_english.json** ，并在文件中加入如下代码：

```json
{
  "title": "Toggle Chinese English With caps_lock",
  "rules": [
    {
      "description": "caps_lock 切换中英文，长按锁定大写，短按恢复小写",
      "manipulators": [
        {
          "type": "basic",
          "from": {
            "key_code": "caps_lock",
            "modifiers": {
              "optional": [
                "left_control"
              ]
            }
          },
          "to_if_alone": [
            {
              "key_code": "spacebar",
              "modifiers": [
                "left_control"
              ]
            }
          ],
          "to_if_held_down": [
            {
              "key_code": "caps_lock"
            }
          ]
        }
      ]
    }
  ]
}
```

**2\. 放置到正确的文件夹下：**

将此文件移动到 **/Users/你的名字/.config/karabiner/assets/complex\_modifications/** 文件夹下，如果没有这个文件夹则创建一个。

![image.png](https://blogimage.houjiyi.com/FqlPUOXKjalyUNKmoWaw9E_c2ibt)

注意 **.config** 是一个隐藏文件夹，访达app 中显示隐藏文件夹的快捷键是 **Shift+Command+.**

**3\. 激活此条规则：**

打开 karabiner elements 软件，并依次点击如下按钮：  
Complex modifications -> Rules -> Add rule

![image.png](https://blogimage.houjiyi.com/FtygfNv9-7AbkHei1jM9kRUYDXc-)

找到 【Toggle Chinese English With caps\_lock】这条规则，并点击后面的 enable 按键。

![image.png](https://blogimage.houjiyi.com/Fh3G-qzF0fBHaZgGrI1b-XpmdUqH)

* * *

## 完成

这样就能使用原生的单一 **caps\_lock** 切换方式切换搜狗拼音等第三方输入法了。若要取消这条规则，则在 **Complex modifications -> Rules** 中找到这条规则，并点击 **remove** 即可。

## 小建议

长按 caps\_lock 触发大写锁定会有一个小问题，那就是没有一个明显的反馈来指示是否触发成功，不好拿捏需要长按多久，这时候需要一个在屏幕上指示大小写锁定状态的软件就可以了。我使用的是 [captin](https://link.zhihu.com/?target=http%3A//captin.mystrikingly.com/) 这款小软件来解决这个问题，下面是它的运行截图。

[Captin on Strikingly​captin.mystrikingly.com/![](https://pic3.zhimg.com/v2-6a4ce874c3bcad1cf569cd7c3a6eea1a_ipico.jpg)](https://link.zhihu.com/?target=http%3A//captin.mystrikingly.com/)

![image.png](https://blogimage.houjiyi.com/FsZDYBQPsQSweYHPXlPImh4UuTiE)

## 题外话

为什么我觉得自带的拼音输入法不好用：

主要是在设定一些快捷键时，当处于原生中文输入法状态下时，一些英文符号按键就会输入成中文符号，导致此时快捷键失效。而搜狗、百度等第三方中文输入法都不会有此问题。如果解决了这个问题，对我而言自带的中文输入法也是够用的。

## 其它问题

1.  如果希望更改 【control + 空格】 快捷键为其它的，则需要同步修改配置文件中的【to\_if\_alone】字段。
