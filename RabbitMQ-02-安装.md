---
title: RabbitMQ-02-安装
categories: [MQ]
tags:
  - MQ
  - backend
date: 2021-09-21 15:36:53
---

## $Linux$安装

安装$Erlang$

$Rabbit\quad MQ$需要$Erlang$的运行环境，所以需要先安装$Erlang$。

在$linux$上安装可以有3种方式

安装$erlang-rpm$包，该包经过MQ官方的处理；

或者使用$Erlang\quad Solutions$源进行安装；

使用$EPEL("Extra\quad Packages \quad For\quad Enterprise\quad Linux")$进行安装，但是版本可能不合适；

<hr/>

这里我使用的第一种方法：

由于$erlang$的镜像$https://dl.bintray.com$在（2021.09.21）已经失效，所以这里可以先下载好rpm包，再安装即可。

```bash
rpm -ivh erlang-22.3.4.21-1.el7.x86_64.rpm --nodeps --force
```

安装完成$erlang$后可以查看版本

![image-20210921165313712](https://gitee.com/cao_ziqiang/img/raw/master/20210921165313.png)

接下来就是安装$Rabbit\quad MQ$

导入密钥

```bash
rpm --import https://www.rabbitmq.com/rabbitmq-release-signing-key.asc
```

接下来安装

```bash
yum install rabbitmq-server-3.8.2.rc.1-1.el7.noarch.rpm 
```

## 启动

```bash
# 启动rabbitmq
systemctl start rabbitmq-server
# 查看rabbitmq状态
rabbitmqctl status
```

![image-20210921170839436](https://gitee.com/cao_ziqiang/img/raw/master/20210921170839.png)

开启web管理界面

```bash
rabbitmq-plugins enable rabbitmq_management
```

添加用户，并设置权限为管理员

```bash
rabbitmqctl add_user admin password
rabbitmqctl set_user_tags admin administrator
```

接下来就可以访问了，web管理界面的默认端口为15672

![image-20210921171846435](https://gitee.com/cao_ziqiang/img/raw/master/20210921171846.png)

## $Docker$安装

还可以尝试$docker$来安装$RabbitMQ$。这样就省去了自己考虑$erlang$版本和$RabbitMQ$版本的兼容问题。

首先更新yum

```shell
# 更新 yum
yum update

# 检查yum依赖的几个包 yum-utils 提供 yum-config-manager 功能， 后面两个是 devicemapper 用到的
yum install -y yum-utils device-mapper-persistent-data lvm2
```

添加阿里云镜像

```shell
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

这里再介绍一些$docker$常用命令

```shell
# 启动 docker
systemctl docker start
# 停止 docker
systemctl docker stop
# 重启 docker
systemctl docker restart
# 查看 docker 状态
systemctl status docker
# 开机自启
systemctl enable docker
systemctl unenable docker

# 导入镜像文件
docker load < xxx.tar.gz
# 查看安装的镜像
docker images
# 删除镜像
docker rmi 镜像名
```

安装$RabbitMQ$

```SH
# 获取 RabbitMQ 的镜像
docker pull rabbitmq:management
# 创建并运行容器
docker run -id --name 容器名 -p 15672:15672 -p 5672:5672 rabbitmq:management
# 安装MQ
docker run -di --name myrabbitmq -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin -p 15672:15672 -p 5672:5672 -p 25672:25672 -p 61613:61613 -p 1883:1883 rabbitmq:management
```

