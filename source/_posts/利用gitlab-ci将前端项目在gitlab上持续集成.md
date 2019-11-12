---
title: 利用gitlab-ci将前端项目在gitlab上持续集成
tags:
  - 开发调试
  - Web开发
categories:
  - Web开发
toc: false
date: 2019-11-07 11:29:06
---

这个需求是这样的，希望代码提交以后能够对应的自动在服务器上拉取新代码，然后打包部署，并支持不同的环境，前端代码我们放在gitlab上，部署在centos7服务器上，具体步骤如下：

<!--more-->
 
## 下载安装git并配置ssh公钥
使用yum下载git：
```bash
yum -y install git 
```
配置ssh key公钥
> 配置ssh公钥是为了在服务器自动获取gitlab上的代码时，可以免输入密码，用https的方式clone的话要手动输入账号密码，这样实现不了想要的自动化效果。

生成ssh公钥
```bash
ssh-keygen -o -t rsa -b 4096 -C "liwenliang@aiforward.com"
```
复制ssh公钥并粘贴到gitlab库
![image.png](http://blogimage.houjiyi.com/FkuXT5CT_GzBYJ5wG76Rs0K5T3TO)

## 克隆一份代码测试可用
添加完成之后，我们直接在linux服务器上直接使用git clone  [SSH url] 指令，来测试刚刚配置的ssh公钥是否有效。注意这里是使用ssh链接clone，用http总要输入密码的。

## 通过yum安装指定版本的nodejs
不要使用nvm等版本工具来安装nodejs，这样在运行时会找不到nodejs，默认安装的nodejs版本太低不符合要求，因此要指定版本来安装，这里我使用10.x版本
```bash
curl --silent --location https://rpm.nodesource.com/setup_10.x | sudo bash -
yum install -y nodejs
npm config set registry https://registry.npm.taobao.org  # 指定淘宝镜像
npm config get registry  # 测试淘宝镜像可用
```

## 安装Gitlab Runner
把gitlab-runner理解成一个服务，他会持续监测仓库的变化，并判断使用哪个runner实例来运行持续集成任务
添加 GitLab's 官方仓库:
```bash
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh | sudo bash
```
下载最新版 GitLab Runner：
```bash
sudo yum install gitlab-runner
```

## 注册gitlab-runner
安装后要进行注册，把注册理解成一个个的runner实例，对应仓库项目里某个特定的集成环境，比如说A项目的prod集成环境，如果机器上有多个集成环境，则需要多次注册。

这里特地查了一下gitlab-runner和gitlab-ci-multi-runner的区别，结论是使用gitlab-runner就行了，后者是前者的一个早期版本，我本地测试也是使用前者就行了

```bash
sudo gitlab-runner register # 注册gitlab-runner实例
```
- GitLab的url：指定gitlab域名
- GitLab的token信息：对应项目的token，URL和token可在gitlab项目的Setting->CI/CD->Runners Setting->Expand中查看
- runner的描述：一个容易记忆的名称，比如“A项目prod环境-xxx服务器”
- runner的标签，用逗号分开：testtag，这个很重要，脚本的tags字段与这里是有对应关系的，用来指定脚本在哪个runner上执行，我们可以使用一定的规范来定义，比如线上环境叫prod，测试环境叫test
- runner的执行者：shell，用来执行runner的程序，这里是shell

> 注册好之后如果在gitlab-ci的界面上看到runner的旁边不是绿点，而是黑色三角的感叹号，可以使用 gitlab-runner start 或者 gitlab-runner run 的指令来开启runner的service
![image.png](http://blogimage.houjiyi.com/Fo-Rr0iUpLZzqtz3fv7PsXAWK-U2)

## 修改gitlab-runner文件夹权限
注册完成后，将gitlab-runner文件夹的用户所属修改为gitlab-runner，这样gitlab-runner对其才会有执行权限。
```bash
sudo chown -R gitlab-runner:gitlab-runner /home/gitlab-runner
```
修改文件读写权限：
```bash
sudo chmod -R 777 /home/gitlab-runner
```
## gitlab-runner用户添加到root组并支持免密使用sudo
由于我们的脚本执行的时候很容易遇到权限问题，因此使用这个方法可以保证脚本执行权限不会有问题
- 添加sudo文件的写权限,命令是:
```bash
chmod u+w /etc/sudoers
```
- 编辑sudoers文件
```bash
vi /etc/sudoers
```
- 找到这行 root ALL=(ALL) ALL,在他下面添加xxx ALL=(ALL) ALL (这里的xxx是你的用户名)

ps:这里说下你可以sudoers添加下面四行中任意一条
> youuser    ALL=(ALL)    ALL
%youuser    ALL=(ALL)    ALL
youuser    ALL=(ALL)    NOPASSWD: ALL
%youuser    ALL=(ALL)    NOPASSWD: ALL
第一行:允许用户youuser执行sudo命令(需要输入密码).
第二行:允许用户组youuser里面的用户执行sudo命令(需要输入密码).
第三行:允许用户youuser执行sudo命令,并且在执行的时候不输入密码.
第四行:允许用户组youuser里面的用户执行sudo命令,并且在执行的时候不输入密码.

- 撤销sudoers文件写权限,命令:
```bash
chmod u-w /etc/sudoers
```
这样普通用户就可以使用sudo了.
## 编写.gitlab-ci.yml文件
这里就不详细解释了，把代码贴在这里吧，其中cache是为了不要每次都重新npm install一遍，tags对应gitlab-runner实例的tag，指定在哪个上边运行。

脚本会默认运行在项目的根目录里边，安装--build--删除原始目录--将新生成的dist移动到对应目录

```bash
cache:
  untracked: true
  key: "$CI_BUILD_REF_NAME"
  paths:
    - node_modules/

stages:
  - deploy

deploy_stage:
  stage: deploy
  script:
    - npm install
    - npm run build
    - sudo rm -rf /home/work/www/fe/ci_cd_demo/dist/
    - sudo mv dist/ /home/work/www/fe/ci_cd_demo/
  only:
    - master
  tags:
    - testtag

```

这里还是要简要说明一个问题，yml文件里的tags指定了gitlab-runner实例所标识的tags，也就是这里指定了要运行哪个实例。

同时还有一个地方需要注意，only不仅可以指定分支，还能够指定标签，这个功能很实用，比如我想在git上[prod-]开头的标签的提交运行我的构建，则需要这样指定即可：
```bash
cache:
  untracked: true
  key: "$CI_BUILD_REF_NAME"
  paths:
    - node_modules/

stages:
  - deploy

deploy_stage:
  stage: deploy
  script:
    - sh ./ci/deploy_prod.sh
  only:
    - /^prod-.*$/
  tags:
    - prod
```


## 测试
最后一步就是提交对应分支的代码，看运行结果了
![image.png](http://blogimage.houjiyi.com/FjqfZmHbNRVXHTFt7sm5HfKwQRUl)