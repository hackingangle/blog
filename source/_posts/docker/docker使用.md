title: docker使用
date: 2017-08-09 20:48:41
tags: [docker]
categories: docker
---
docker这些年非常火，我会在这里给大家介绍docker的常用命令，让大家游刃有余的操作docker。
<!-- more -->

## what

https://yeasy.gitbooks.io/docker_practice/content/install/mac.html

## how

- 拉取镜像
    - docker pull pangee/lnmp:v1
- 查看当前镜像
    - docker images

``` shell
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
pangee/lnmp         v1                  fe7922893d3e        3 weeks ago         9.29GB
```

- 开启一个容器，利用当前某一个镜像
    - docker run -d -p 8001:8001 -p 8000:8000 -p 80:80 fe /sbin/init
        - 此处某些同学会懵逼，为什么用`fe`就指定了一个镜像，我想说请看前面`docker images`执行结果中的`IMAGE ID`这个属性，用前面几个字母唯一标示则可，大赞这个功能
        - `-d`是后台启动
        - `-p`是端口映射
- 查看容器状态
    - `docker stats`

``` shell
CONTAINER           CPU %               MEM USAGE / LIMIT   MEM %               NET I/O             BLOCK I/O           PIDS
6cb5718fff15        0.00%               488KiB / 995.8MiB   0.05%               828B / 0B           20.5kB / 69.6kB     1
```

- 连接容器
    - `docker exec -it 6 /bin/bash`
        - 代表的含义也是唯一的标示，与`IMAGE ID`用法一致，大赞
