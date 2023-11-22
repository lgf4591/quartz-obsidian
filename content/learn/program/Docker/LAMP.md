---
aliases: [LAMP ]
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:53:36
tags: [learn/program/docker, learn/program/common]
title: LAMP 
---
# LAMP
## 章节介绍

> houdunren.com @ 向军大叔

![xj-small](https://doc.houdunren.com/assets/img/xj.161cc3f2.jpg)

本章我们通过配置 LNMP 环境，学习镜像配置与容器编排，最终在环境中可以良好运行 LARAVEL 框架，同时使用代理转发构建单一服务器构建多个 DOCKER 网站。

本章节内容已经创建独立项目，请访问 [GITHUB (opens new window)](https://github.com/houdunwang/docker) 或 [GITEE (opens new window)](https://gitee.com/houdunren/docker) 查看

## 安装配置

下面来配置 Docker 运行环境

### 安装 Docker

首先访问 [Docker Hub (opens new window)](https://hub.docker.com/) 下载安装 Docker

### 宿主机

完成本章前要注意以下几点

1.  关闭服务器防火墙
    
2.  关闭宿主机 NGINX | APACHE 服务，因为有可能设置和宿主相同端口，造成运行时不确定在改宿主还是容器
    

## 国内镜像

配置 Docker 国内镜像可以提高下载镜像的速度

### 国内镜像

下面是可以使用的 docker 国内镜像

### Mac

访问 docker 软件 `设置 > Docker Engine` 进行配置

![image-20210531191211984](https://doc.houdunren.com/assets/img/image-20210531191211984.3f44d5c3.png)

### Linux

Linux 需要修改`/etc/docker/daemon.json` docker 配置文件，并添加以下内容

## 目录结构

下面是实验的文件结构，便于有个全局认识

## DOCKFILE

Dockerfile 是由一系列命令和参数构成的脚本，这些命令应用于基础镜像并最终创建个性化的新镜像。

### PHP

先可以查看一下官方镜像都安装了哪些扩展，下面命令会下载镜像并进入到新生成的容器中

在容器中执行命令可以查看到已经内置的模块，这里没有的模块就需要后面安装了

#### 配置文件

下面是 PHP 的 dockfile 配置文件内容

**说明**

1.  RUN 指令是在镜像构建时执行，一般用于安装软件包
2.  安装软件包最好加上版本号，默认使用最新版本，这可能会造成以后再编译时和新版本不兼容
3.  如果编译速度慢，可以把上面注释的修改镜像国内源打开

#### 编译镜像

下面是对单个镜像的编译，其实使用 `docker-compose` 可以在编译镜像时同时编译，但下面还是了解一下编译命令的使用。

在 Dockerfile 文件所在目录执行编译镜像操作，-t 用于设置镜像名称

也可以为镜像设置版本号，默认的版本是 `latest`

查看镜像编译是否成功

### NGINX

下面是 NGINX 的 `nginx/Dockerfile` 配置

下面设置虚拟主机配置文件 `nginx/config/default.conf`，设置了两个域名 `hdcms.test` 与 `houdunren.test`

### MYSQL

mysql 的 `mysql/Dockerfile` 配置不需要过多配置

## COMPOSE

使用 dock-compose 可以统一编排容器，减少容器单独启动的麻烦，同时也可以将配置一次定义，方便在不同服务器中利用。

### 配置文件

**.env** 配置文件用于定义配置变量，在 `docker-compose.yaml` 文件中使用，简化修改大文件的麻烦。

下面是 `docker-compose.ymal` 配置文件用来编排容器，放在项目的根目录（请看上面目录结构）

**注意事项**

因为 NGINX 与 PHP 需要通过容器名来通信，所以如果修改了容器名称需要修改 `nginx/config/default` 文件，将配置文件中的 `houdunren-php` 修改为新的 PHP 容器名称。

### 项目配置

系统包括 NGINX、PHP 等软件的项目配置文件，修改这些配置文件不需要重新编译，只需要在`docker-compose.yaml`文件所在目录下重起容器就可以了。

### 执行构建

使用以下命令启动构建

如果修改过 `docker-compose.yaml`配置文件，再次执行命令即可重新构建容器

## 容器访问

经过编排的过镜像已经在一个网络中，互相可以使用容器名来访问，这是后面做代理转发的前提。

下面是在`houdunren-php` 容器中来连接 `houdunren-nginx` 容器的试验

1.  查看容器列表
    
2.  进入 houdunren-php 容器
    
3.  来 PING 容器 houdunren-nginx
    

## 域名解析

本地进行测试时需要修改 `hosts` 添加域名解析

## LARAVEL

下面来安装 LARAVEL 项目，你可以安装任何其它 PHP 项目来使用，具体可以查看[后盾人 (opens new window)](https://www.houdunren.com/)在线文档或视频学习 LARAVEL 的安装使用。

因为 LARAVEL 要解析到**public**目录，修改 NGINX 配置文件 `nginx/config/default.conf` 目录相关内容

修改配置后需要生起容器服务

现在访问就可以看到 LARAVEL 欢迎页面了

![image-20200112124436521](https://doc.houdunren.com/assets/img/image-20200112124436521.2641a9e9.png)

## 数据库连接

下面我们使用 MYSQL 管理 GUI 工具 DBeaver 连接容器数据库，默认 MYSQL 端口是 33060 可以在.env 文件中修改。

如果修改了.env 中的配置需要重新编译容器

使用 DBeaver 访问

![image-20200112130002715](https://doc.houdunren.com/assets/img/image-20200112130002715.a914691a.png)
