---
title: Linux 网络速度测试工具
date: 2017-12-26 15:28:52
tags: [Linux, 网速]
---

只需要安装一个工具即可:

```
wget https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py

chmod a+rx speedtest.py

mv speedtest.py /usr/local/bin/speedtest-cli

chown root:root /usr/local/bin/speedtest-cli

```