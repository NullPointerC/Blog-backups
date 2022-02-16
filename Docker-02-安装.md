---
title: Docker-02-安装
categories:
  - Docker
tags:
  - Docker
  - Linux
abbrlink: 54ad4777
date: 2021-09-20 16:50:40
---

## Docker安装

首先来到一台新的$CentOS$系统的主机上。注意以下命令都要以$root$权限的用户进行操作。

1. 更换$yum$源。由于我们需要使用$yum$。所以先更换$yum$为国内的源。

```bash
wget  /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
```

2. 提示安装完成后，则卸载原来的$yum$包并重新下载$yum$

```bash
yum clean all
yum makecache
```

3. 卸载旧的$docker$版本。为防止有些服务器厂商出厂自带$docker$。

```bash
yum remove docker \
	docker-client \
	docker-client-latest \
	docker-common \
	docker-latest \
	docker-latest-logrotate \
	docker-logrotate \
	docker-engine
```

如果$yum$报告没有安装这些软件包，也没有问题。

![image-20210920171106355](https://gitee.com/cao_ziqiang/img/raw/master/20210920171106.png)

4. 更新$yum$

```bash
yum check-update
yum update
```

![image-20210920173043128](https://gitee.com/cao_ziqiang/img/raw/master/20210920173043.png)

再次输入来查看是否全部更新完毕

![image-20210920173455398](https://gitee.com/cao_ziqiang/img/raw/master/20210920173455.png)

如上所示提示没有需要更新的软件包即可。

6. 安装$docker$所需要的软件包

```bash
yum install -y yum-utils \
	device-mapper-persistent-data
	lvm2
```

7. 安装所需软件包后再指定稳定的存储库

```bash
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

![image-20210920174221127](https://gitee.com/cao_ziqiang/img/raw/master/20210920174221.png)

8. 查看$docker$版本

```bash
yum list docker-ce --showduplicates | sort -r
```

![image-20210920174359974](https://gitee.com/cao_ziqiang/img/raw/master/20210920174400.png)

9. 安装指定版本的$docker$

```bash
yum install docker-ce-18.09.0 docker-ce-cli-18.09.0 containerd.io
```

![image-20210920174731998](https://gitee.com/cao_ziqiang/img/raw/master/20210920174732.png)

到这里$docker$就安装好了。由于$docker$也是$C/S$架构的软件，所以接下来就是启动$docker$

10. 启动$docker$

```bash
systemctl start docker
```

再次输入

```dockerfile
docker version
docker info
```

都可以查看$docker$的信息。

![image-20210920175203320](https://gitee.com/cao_ziqiang/img/raw/master/20210920175203.png)

## Docker容器

首先要下载镜像

```dockerfile
docker pull [OPTIONS] NAME[:TAG]
```

查看镜像

```dockerfile
docker images [OPTIONS] [REPOSITORY][:TAG]
```

![image-20210920175653867](https://gitee.com/cao_ziqiang/img/raw/master/20210920175653.png)

运行镜像

```dockerfile
docker run [OPTIONS] IMAGE [COMMAND][ARG...]
```

![image-20210920180005442](https://gitee.com/cao_ziqiang/img/raw/master/20210920180005.png)

## Docker运行Nginx

这里推荐网易的镜像中心：[link](https://c.163yun.com/hub#/home)

<hr/>

我们输入$nginx$。

![image-20210920180801633](https://gitee.com/cao_ziqiang/img/raw/master/20210920180801.png)

这里有两个版本，一个是$library$，一个是$public$。这两个一个是$docker$官方，一个是网易云提供的，这里两个都可以选择，我们选择$library$使用。

![image-20210920180911738](https://gitee.com/cao_ziqiang/img/raw/master/20210920180911.png)

根据这里的下载地址和版本指定下载的命令

```dockerfile
docker pull hub.c.163.com/library/nginx:1.13.0
```

![image-20210920181024559](https://gitee.com/cao_ziqiang/img/raw/master/20210920181024.png)

拉取完毕后，启动这个镜像。

```dockerfile
docker run -d hub.c.163.com/library/nginx:1.13.0
```

![image-20210920181339048](https://gitee.com/cao_ziqiang/img/raw/master/20210920181339.png)

由于$nginx$是一个服务器后台进程，所以加上-d指令使其后台运行。

运行后会返回$docker$容器的$id$。

再使用

```dockerfile
docker ps
```

查看$docker$的状态

![image-20210920181529185](https://gitee.com/cao_ziqiang/img/raw/master/20210920181529.png)

进入容器

```dockerfile
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
```

这里的$CONTAINER$只作为一个标识分别不容容器，不必全部输完全。

```dockerfile
docker exec -it 5a bash
```

![image-20210920181855663](https://gitee.com/cao_ziqiang/img/raw/master/20210920181855.png)

进入容器内部后查看$nginx$的位置以及文件等，可以看到$docker$虚拟化出了一样的文件结构。

![image-20210920182110023](https://gitee.com/cao_ziqiang/img/raw/master/20210920182110.png)