---
title: mkdir 用法
tags:
  - linux
originContent: "这次主要想记录的是\n\n```\nmkdir -p folder1/folder2/folder3  # 没有文件夹则创建文件夹\n```\n\n帮助文档可以看下边：\n\n```\n用法：mkdir [选项]... 目录...\nCreate the DIRECTORY(ies), if they do not already exist.\n\nMandatory arguments to long options are mandatory for short options too.\n  -m, --mode=MODE   set file mode (as in chmod), not a=rwx - umask\n  -p, --parents     no error if existing, make parent directories as needed\n  -v, --verbose     print a message for each created directory\n  -Z                   set SELinux security context of each created directory\n                         to the default type\n      --context[=CTX]  like -Z, or if CTX is specified then set the SELinux\n                         or SMACK security context to CTX\n      --help\t\t显示此帮助信息并退出\n      --version\t\t显示版本信息并退出\n\nGNU coreutils online help: <http://www.gnu.org/software/coreutils/>\n请向<http://translationproject.org/team/zh_CN.html> 报告mkdir 的翻译错误\n要获取完整文档，请运行：info coreutils 'mkdir invocation'\n\n```"
categories:
  - 后端开发
toc: false
date: 2018-01-02 14:41:08
---

这次主要想记录的是

```
mkdir -p folder1/folder2/folder3  # 没有文件夹则创建文件夹
```

帮助文档可以看下边：

```
用法：mkdir [选项]... 目录...
Create the DIRECTORY(ies), if they do not already exist.

Mandatory arguments to long options are mandatory for short options too.
  -m, --mode=MODE   set file mode (as in chmod), not a=rwx - umask
  -p, --parents     no error if existing, make parent directories as needed
  -v, --verbose     print a message for each created directory
  -Z                   set SELinux security context of each created directory
                         to the default type
      --context[=CTX]  like -Z, or if CTX is specified then set the SELinux
                         or SMACK security context to CTX
      --help		显示此帮助信息并退出
      --version		显示版本信息并退出

GNU coreutils online help: <http://www.gnu.org/software/coreutils/>
请向<http://translationproject.org/team/zh_CN.html> 报告mkdir 的翻译错误
要获取完整文档，请运行：info coreutils 'mkdir invocation'

```