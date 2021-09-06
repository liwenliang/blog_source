---
title: 微前端架构下腾讯云K8S的部署方式
tags: []
categories: []
toc: false
date: 2021-05-06 17:05:14
---

# 微前端架构下腾讯云K8S的部署方式

## 总体思路

> 每个应用都对应一个镜像，这个镜像有版本的概念，每次打包都是创建一个新版本

> K8S存在集群的概念，不同环境可以有不同的集群，比如预发布环境可以有3台机器和生产环境可以有7台机器
![image.png](http://blogimage.houjiyi.com/Fnr9oyazc53y7ZA9caBjP-f8LKWB)
<!-- more -->
> 进入集群里边，前端应用创建`工作负载`Deployment，对应的就是一个负载对应一个镜像，其中`选择镜像版本`相当于上线不同的代码，每次有新的上线实际上就是更新这里的最新版本；
![image.png](http://blogimage.houjiyi.com/Fj_1xuC02ISX0roQv5z1O9nUXBYN)

> `服务与路由`中的Service，在每个deployment创建以后会自动创建一个，他会生成IP地址，方便我们的域名解析到对应到deployment；
> Ingress 是允许访问到集群内 Service 规则的集合，您可以通过配置转发规则，实现不同 URL 可以访问到集群内不同的 Service；里边还能配置http和https证书；
![image.png](http://blogimage.houjiyi.com/Fv8DsXWA1WK1Ho0Bg_Cw6aoVwO2_)

> 上边是一些概念，如果不实际使用的话，很难去理解他们之间的关系；也就是会者不难、难者不会的一个情况；当你需要的时候自然会去认识、了解、操作，之后就知道是怎么回事了。这里讲一个地方即可就是我们的项目需要打包成镜像，然后发布到K8S上，实际上发布到K8S可以交给运维同学来做，我们需要做的是打包这块的流程，因为这里运维无法帮助你；

## 用于打包的镜像
- 我们需要有两个镜像，一个是用于打包的镜像，一个是用于部署的镜像
- 用于部署的镜像比较简单，一个nginx服务镜像即可；
- 用于打包的镜像相对复杂些，需要有nodejs环境，python环境（用于上传静态资源）
- 打包的镜像拉到本地后，将当前目录下的文件全部拷贝进去，然后安装及打包，这样打包的就是当前分支的代码，也就是说不用指定分支，使用当前分支即可
- 打包后将dist文件上传到对应的项目目录下，这样静态资源就可以访问了，这里的上传用到了腾讯云的python上传工具：https://cloud.tencent.com/document/product/436/10976
- 还有一个镜像是用来部署环境的，这里直接使用一个dockerhub上的nginx镜像即可

## 看下脚本代码
```bash
#!/bin/bash

set -e

# 1 build 基础
# 2. 日常
script_name=`basename $0`
build_type=$1
option=$2

usage() {
    echo "${script_name} [build_type prod=<正式环境> | pre=<灰度环境> | test=<测试环境> | dev=<开发环境> | base=<基础配置>]"
}

DOCKER_FILE="Dockerfile"
IMAGE_NAME="fe_fpms_microapp_prod"
case $build_type in
pre)
    IMAGE_NAME="fe_fpms_microapp_pre"
    ;;
test)
    IMAGE_NAME="fe_fpms_microapp_test"
    ;;
dev)
    IMAGE_NAME="fe_fpms_microapp_dev"
    ;;
base)
    DOCKER_FILE="build.Dockerfile"
    IMAGE_NAME="fe_fpms_microapp_build"
    ;;
prod)
    ;;
*)
    usage
    exit 1
    ;;
esac

IMAGE_PREFIX=ccr.ccs.tencentyun.com/aiforward_saas_fe/${IMAGE_NAME}
IMAGE_TAG=`date +'%Y%m%d%H%M'`-`git rev-parse --short HEAD`
IMAGE=$IMAGE_PREFIX:${IMAGE_TAG}
echo "------------------------------"
echo "Make sure docker login first!"
echo "Run the followings..."
echo "docker login ccr.ccs.tencentyun.com"
echo ""
echo "Git branch: "`git rev-parse --abbrev-ref HEAD`
echo "Git SHA   : "`git rev-parse HEAD`
echo "Image     : ${IMAGE}"
echo ""
echo "Press any to continue, Ctrl + C to cancel"
read p


echo "=> Buiding image: $IMAGE And build_type=$build_type"
docker build -t $IMAGE . -f $DOCKER_FILE --build-arg build_type=$build_type
echo "=> Pushing image: $IMAGE"
docker push $IMAGE

```
> Dockerfile
```bash

FROM ccr.ccs.tencentyun.com/aiforward_saas_fe/fe_fpms_build:202104290107  AS builder

ARG build_type=prod
# 每一个项目都不同 fpms_pro 根据vue.config.js中的publicpath而来
ARG tencent_path_name=fpms_microapp
WORKDIR /app/
COPY . /app/

RUN npm install --registry=https://registry.npm.taobao.org
RUN npm run build:$build_type
RUN /usr/local/bin/coscmd upload -rsf /app/dist fe/src/$tencent_path_name/dist

FROM nginx:stable-alpine

ADD ./misc/default.conf /etc/nginx/conf.d/default.conf
ADD ./misc/nginx.conf /etc/nginx/nginx.conf
COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

- 打包镜像-fe_fpms_build:202104290107，它是我们自定义的打包环境镜像，因此每次打包都使用它
- 部署镜像-nginx:stable-alpine
- 打包镜像是自己通过其他镜像创建container，然后再进入container安装打包环境，再把container打包成镜像并推送到腾讯云（ccr.ccs.tencentyun.com/aiforward_saas_fe/fe_fpms_build:202104290107）

> 这些都准备好以后，我们每次进行上线，只需要本地打包以后然后推送到腾讯云上即可，再到腾讯云修改部署的版本。
> 这里腾讯云的登录 `docker login ccr.ccs.tencentyun.com`