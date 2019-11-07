---
title: How To Install Jenkins on CentOS 7
tags:
  - jenkins
categories:
  - Web开发
toc: false
date: 2019-08-14 13:39:31
---

![image.png](http://blogimage.houjiyi.com/FqxHz-9hRWp535eOZdqOoFW0ysEz)

```bash
sudo yum install java-1.8.0-openjdk-devel

curl --silent --location http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo | sudo tee /etc/yum.repos.d/jenkins.repo

sudo rpm --import http://pkg.jenkins.io/redhat-stable/jenkins.io.key

sudo yum install jenkins

sudo systemctl start jenkins

sudo systemctl enable jenkins

```
