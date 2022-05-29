---
title: Docker
date: 2022/5/5 11:21:25
tags:
  - Docker
  - 项目部署
categories:
  - 项目部署
cover: /img/docker/docker.jpg
---



# Docker

## 1. 概述

一款产品：开发-上线：两套环境（应用环境和应用配置）

开发 -- 运维 

`问题`：版本更新导致服务不可用！

 发布一个项目，能不能带着环境打包。



java -- apk -- 发布（应用商店）-- 张三使用apk -- 安装即可用

java -- jar（环境）-- 打包项目带上环境（镜像）-- Docker仓库：商店 -- 下载我们发布的镜像 -- 直接运行即可



## 2. 历史

###### 物理机部署时代 

> 进程间资源抢占。如果某个进程耗用了100%的CPU资源，其他的进程无法提供服务。或者如果一个进程因为突发异常很多，日志把磁盘打满了，所有的进程都要挂掉。进程间抢占资源导致其他进程无法提供服务所导致的血案数不胜数。

###### 虚拟机部署时代--解决资源共享问题

> 软件版本和配置文件容易碎片化 
>
> 当线上出问题，需要排查到基础软件层面时，由于软件版本碎片化问题，导致排查变得棘手

###### 容器化时代

> 容器如果要升级软件版本，那就修改镜像文件。这样部署时集群内所有的机器重新拉取新的镜像，软件因此跟着一起升级。
>
> 有了容器技术，生产环境为何还需要部署虚拟机

## 3. 能做什么

###### 虚拟机

![image-20220505170851702](C:/Users/Megumi/AppData/Roaming/Typora/typora-user-images/image-20220505170851702.png)

缺点：

- 资源占用十分多
- 冗余步骤多
- 启动很慢！

###### 容器化技术

> 并不是模拟一个完整的操作系统

![image-20220505171127157](C:/Users/Megumi/AppData/Roaming/Typora/typora-user-images/image-20220505171127157.png)

比较docker和虚拟机技术的不同：

- 传统虚拟机，虚拟出一条硬件，运行一个完整的操作系统，然后再这个系统上安装和运行软件
- 容器内的应用直接运行再宿主机的内容，容器是没有自己的内核的，也没有虚拟我们的硬件，所以就轻便
- 每个容器见是相互隔离的，都有一个属于自己的文件系统不互相影响

###### DevOps

> 应用更快速的交付和部署

传统：一堆帮助文档，安装程序

Docker：打包镜像发布测试，一键运行

> 更便捷的升级和拓缩容

使用了Docker之后，部署应用和搭积木一样

> 更简单的系统运维

在容器化之后，我们的开发，测试环境都是高度一致的

> 更高效的计算资源利用

Docker是内核级别的虚拟化

## 4. Docker安装

![image-20220505172854323](C:/Users/Megumi/AppData/Roaming/Typora/typora-user-images/image-20220505172854323.png)

###### 镜像：

docker镜像就好比是一个模板，可以通过这个模板来创建容器服务，tomcat镜像==》run==》tomcat容器，通过这个镜像可以创建多个容器，最终服务运行实在容器中的

###### 容器：

Docker利用容器技术，独立运行一个或者一组应用，通过镜像来创建

目前可以把容器理解为就是一个简单的linux系统

###### 仓库：

仓库就是存放镜像的地方

`配置镜像加速！`

###### Docker安装

```linux
// 卸载旧的版本
yum remove docker
// 需要的安装包
yum install -y yum-utils
// 设置镜像的仓库
yum-config-manager \
--add-repo \
http://mirros.aliyun.com/docker-ce/linux/centos/docker-ce.repo
// 安装最新的docker引擎
yum install docker-ce docker-ce-cli containerd.io
// 启动docker
systemctl start docker
// 查看docker
docker version
// docker run

// 查看镜像
docker image
// 卸载docker
yum remove docker-ce docker-ce-cli containerd.io
rm -rf /var/lib/docker (docker 默认资源路径)

```

![image-20220506091246023](C:/Users/Megumi/AppData/Roaming/Typora/typora-user-images/image-20220506091246023.png)

## 5. 回顾helloworld

![image-20220506092154916](C:/Users/Megumi/AppData/Roaming/Typora/typora-user-images/image-20220506092154916.png)

## 6. 底层原理

###### Docker是怎么工作的

docker是一个Client-Server结构的系统，Docker的守护进程运行在主机上，通过Socket从客户端访问，DockerServer接受到Docker-Client的指令，就会执行这个命令！

###### 核心技术

> `Linux上的名字空间`：
>
> ​	pid名字空间，隔离进程、net隔离网络（独立的网络设备，IP地址，路由表）
>
> ​	ipc名字空间（进程间的交互方法：包括信号量、消息队列和共享内存）docker只满足相同pid之间进行交互
>
> ​	mnt名字空间：将一个进程放进一个特定的目录执行，允许不同mnt看见的目录结构不同。
>
> ​	uts名字空间：允许每个容器又独立的hostname和domain name，视为一个独立的结点，而非一个进程
>
> ​	user名字空间：每个容器可以又不同的用户和组id
>
> `控制组`：
>
> ​	主要用来对共享资源进行隔离、限制、审计等。只有能控制分配到容器的资源，才能避免当多个容器同时运行时的对系统资源的竞争。
>
> `Union文件系统`：
>
> ​	是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统
>
> `容器格式`
>
> ​	Docker 采用了 LXC 中的容器格式。自 1.20 版本开始，Docker 也开始支持新的 libcontainer 格式，并作为默认选项。

## 7. Docker的网络实现

Docker的网络实现就是利用了Linux上的网络名字空间和虚拟网络设备

###### 基本原理

`网络接口`：虚拟的接口（转发效率较高）

`路由机制`：在内核中进行数据复制来实现虚拟接口之间的数据转发，发送接口的发送缓存中的数据包直接被复制到接受接口的接收缓存中。对于本地系统和容器内系统看来就像是一个正常的以太网卡。不需要真正痛外部通信速度快得多。`docker利用了这项技术，在本地主机和容器内分别创建了一个虚拟接口，并让他们彼此连通，这样的一对接口叫做veth pair`

![image-20220517110005033](D:\code\BLog\source\_posts\image-20220517110005033.png)

`docker run --net`

- --net=bridge 连接到默认的网桥
- host 连接到主机
- container 连接到已存在的网络栈
- none 展示不连接

## 8. Docker file

###### 基本结构

由一行行命令语句组成，并且支持#开头

`四部分`

- 基础镜像信息

- 维护者信息

- 镜像操作指令

  后面则是镜像操作指令，例如 RUN 指令，RUN 指令将对镜像执行跟随的命令。每运行一条 RUN 指令，镜像添加新的一层，并提交。

- 容器启动时执行指令

```dockerfile
# This dockerfile uses the ubuntu image
# VERSION 2 - EDITION 1
# Author: docker_user
# Command format: Instruction [arguments / command] ..

# Base image to use, this must be set as the first line
FROM ubuntu

# Maintainer: docker_user <docker_user at email.com> (@docker_user)
MAINTAINER docker_user docker_user@email.com

# Commands to update the image
RUN echo "deb http://archive.ubuntu.com/ubuntu/ raring main universe" >> /etc/apt/sources.list
RUN apt-get update && apt-get install -y nginx
RUN echo "\ndaemon off;" >> /etc/nginx/nginx.conf

# Commands when creating a new container
CMD /usr/sbin/nginx
```

`指令`

- FROM

  第一条指令必须为FROM指令，并且在同一个Dockerfile中创建多个镜像时，可以使用多个FROM，每个镜像一次

- MAINTAINER

  指定维护者信息

- RUN

  每条 RUN 指令将在当前镜像基础上执行指定命令，并提交为新的镜像。当命令较长时可以使用 \ 来换行

- CMD

  指定启动容器时执行的命令，每个 Dockerfile 只能有一条 CMD 命令。如果指定了多条命令，只有最后一条会被执行。

- EXPOSE

  告诉 Docker 服务端容器暴露的端口号，供互联系统使用。在启动容器时需要通过 -P，Docker 主机会自动分配一个端口转发到指定的端口。

- ENV

  格式为 ENV 。 指定一个环境变量，会被后续 RUN 指令使用，并在容器运行时保持。

```dockerfile
ENV PG_MAJOR 9.3
ENV PG_VERSION 9.3.4
RUN curl -SL http://example.com/postgres-$PG_VERSION.tar.xz | tar -xJC /usr/src/postgress && …
ENV PATH /usr/local/postgres-$PG_MAJOR/bin:$PATH
# Nginx
#
# VERSION               0.0.1

FROM      ubuntu
MAINTAINER Victor Vieux <victor@docker.com>

RUN apt-get update && apt-get install -y inotify-tools nginx apache2 openssh-server

# Firefox over VNC
#
# VERSION               0.3

FROM ubuntu

# Install vnc, xvfb in order to create a 'fake' display and firefox
RUN apt-get update && apt-get install -y x11vnc xvfb firefox
RUN mkdir /.vnc
# Setup a password
RUN x11vnc -storepasswd 1234 ~/.vnc/passwd
# Autostart firefox (might not be the best way, but it does the trick)
RUN bash -c 'echo "firefox" >> /.bashrc'

EXPOSE 5900
CMD    ["x11vnc", "-forever", "-usepw", "-create"]

# Multiple images example
#
# VERSION               0.1

FROM ubuntu
RUN echo foo > bar
# Will output something like ===> 907ad6c2736f

FROM ubuntu
RUN echo moo > oink
# Will output something like ===> 695d7793cbe4

# You᾿ll now have two images, 907ad6c2736f with /bar, and 695d7793cbe4 with
# /oink.
```

- ADD

  该命令将复制指定的 到容器中的 。 其中 可以是Dockerfile所在目录的一个相对路径；也可以是一个 URL；还可以是一个 tar 文件（自动解压为目录）。

- COPY

  复制本地主机的 （为 Dockerfile 所在目录的相对路径）到容器中的 。

  当使用本地目录为源目录时，推荐使用 COPY。

- VOLUME

  创建一个可以从本地主机或其他容器挂载的挂载点，一般用来存放数据库和需要保持的数据等。

- USER

  指定运行容器的用户名或UID

- WORKDIR

  为后续的RUN、CMD、ENTRYPOINT指令配置工作目录

  可以使用多个 WORKDIR 指令，后续命令如果参数是相对路径，则会基于之前命令指定的路径。例如

  ```dockerfile
  WORKDIR /a
  WORKDIR b
  WORKDIR c
  RUN pwd
  ```

- ONBUILD

  配置当前的镜像作为别的镜像的基础镜像

  ```dockerfile
  [...]
  ONBUILD ADD . /app/src
  ONBUILD RUN /usr/local/bin/python-build --dir /app/src
  [...]
  ```

  ```dockerfile
  FROM image-A
  
  #Automatically run the following
  ADD . /app/src
  RUN /usr/local/bin/python-build --dir /app/src
  ```

###### 创建镜像

基本的格式为 docker build [选项] 路径，

该命令将读取指定路径下（包括子目录）的 Dockerfile

并将该路径下所有内容发送给 Docker 服务端，由服务端来创建镜像。

因此一般建议放置 Dockerfile 的目录为空目录。也可以通过 .dockerignore 文件（每一行添加一条匹配模式）来让 Docker 忽略路径下的目录和文件。

```shell
sudo docker build -t myrepo/myapp /tmp/test1/
```

## 9. docker安装

环境centos7.*

## 10. 运行helloWorld

###### 下载Ubuntu14.02镜像

```shell
sudo docker run ubuntu:14.04 /bin/echo 'Hello world'
```

###### 交互式的容器

```shell
$ sudo docker run -t -i ubuntu:14.04 /bin/bash
root@af8bae53bdd3:/#
```

但是我们也加了两个新的标示：`-t`和`-i`。`-t`表示在新容器内指定一个伪终端或终端，`-i`表示允许我们对容器内的STDIN进行交互。

###### 退出容器

```shell
root@af8bae53bdd3:/# exit
```

###### 守护进程

```shell
$ sudo docker run -d ubuntu:14.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"
1e5535038e285177d5214659a068137486f96ee5c2e85a4ac52dc83f2ebe4147
```

为什么不是我们看到的一大堆的”hello word”?而是docker返回的一个很长的字符串：

```
1e5535038e285177d5214659a068137486f96ee5c2e85a4ac52dc83f2ebe4147
```

这个长的字符串叫做`容器ID`。它是容器的唯一标识，所以我们可以使用它。

![image-20220517135354600](D:\code\BLog\source\_posts\image-20220517135354600.png)

###### 终止docker

```shell
sudo docker stop intelligent_aryabhata
```

## 11. Docker安装程序

###### 创建Dockerfile

```shell
nano Dockerfile
```

###### 构建Dockerfile文件

```dockerfile
# Format: FROM    repository[:version]
FROM       ubuntu:latest
# Format: MAINTAINER Name 
MAINTAINER M.Y. Name 
# Installation:
# Import MongoDB public GPG key AND create a MongoDB list file
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
RUN echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | tee /etc/apt/sources.list.d/10gen.list
# Update apt-get sources AND install MongoDB
RUN apt-get update
RUN apt-get install -y -q mongodb-org
# Create the MongoDB data directory
RUN mkdir -p /data/db
# Expose port 27017 from the container to the host
EXPOSE 27017

# Set usr/bin/mongod as the dockerized entry-point application
ENTRYPOINT usr/bin/mongod
```

###### 根据DockerFile构建镜像

```shell
# Format: sudo docker build --tag/-t / .
# Example:
$ sudo docker build --tag my/repo .
```

###### 推送到DockerHub上做分享

```Shell
sudo docker login

sudo docker push my/repo
```

###### 使用MongoDB镜像

```Shell
sudo docker run --name mongo_instance_001 -d my/repo

sudo docker run --name mongo_instance_001 -d my/repo --noprealloc --smallfiles

sudo docker logs mongo_instance_001
```

