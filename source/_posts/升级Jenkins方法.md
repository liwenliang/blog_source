---
title: 升级Jenkins方法
tags:
  - jenkins
  - docker
originContent: "今天打开jenkins提示可以升级，于是按照提示点击下载，得到一个war包，因为使用的docker环境，这时候进入到对应的docker容器：\n\n<!-- more -->\n\n1. 进入到jenkins容器：nsenter --target [容器pid] --mount --uts --ipc --net --pid\n\n2. ps -aux | grep jenkins   查看jenkins安装路径\n\n\t```\n\t    jenkins      1  0.0  0.0   3144   244 ?        Ss   22:12   0:00 /bin/tini -- /usr/local/bin/jenkins.sh   \n\t    jenkins      5  4.7 19.8 2317472 201948 ?      Sl   22:12   0:35 java -jar /usr/share/jenkins/jenkins.war\n\t    root        163  0.0  0.0  13152   704 ?       S+   22:25   0:00 grep jenkins                             \n\t```\n\n3. cd /usr/share/jenkins/   进入jenkins目录\n\n4. cp jenkins.war jenkins.war.bak   备份一下，出问题时可以恢复\n\n5. wget wget https://mirrors.tuna.tsinghua.edu.cn/jenkins/war-stable/2.60.3/jenkins.war   这个链接地址就是jenkins里提示的下载按钮的链接地址\n\n6. 删除原来的jenkins.war 下载下来的是jenkins.war.1 重命名为jenkins.war即可：\n\n\t```\n\trm jenkins.war; \n\tmv jenkins.war.1 jenkins.war\n\t```\n\n7. 完成之后exit退出容器，重启容器，docker restart [容器id]"
categories:
  - 后端开发
toc: false
date: 2017-08-27 06:17:10
---

今天打开jenkins提示可以升级，于是按照提示点击下载，得到一个war包，因为使用的docker环境，这时候进入到对应的docker容器：

<!-- more -->

1. 进入到jenkins容器：nsenter --target [容器pid] --mount --uts --ipc --net --pid

2. ps -aux | grep jenkins   查看jenkins安装路径

	```
	    jenkins      1  0.0  0.0   3144   244 ?        Ss   22:12   0:00 /bin/tini -- /usr/local/bin/jenkins.sh   
	    jenkins      5  4.7 19.8 2317472 201948 ?      Sl   22:12   0:35 java -jar /usr/share/jenkins/jenkins.war
	    root        163  0.0  0.0  13152   704 ?       S+   22:25   0:00 grep jenkins                             
	```

3. cd /usr/share/jenkins/   进入jenkins目录

4. cp jenkins.war jenkins.war.bak   备份一下，出问题时可以恢复

5. wget wget https://mirrors.tuna.tsinghua.edu.cn/jenkins/war-stable/2.60.3/jenkins.war   这个链接地址就是jenkins里提示的下载按钮的链接地址

6. 删除原来的jenkins.war 下载下来的是jenkins.war.1 重命名为jenkins.war即可：

	```
	rm jenkins.war; 
	mv jenkins.war.1 jenkins.war
	```

7. 完成之后exit退出容器，重启容器，docker restart [容器id]