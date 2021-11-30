---
title: Docker-03-访问
categories: [Docker]
tags:
  - Docker
  - Linux
date: 2021-09-20 19:19:14
---

## $Docker$网络模式

### $bridge$

桥接模式，通常通过端口映射技术使外部访问

```dockerfile
docker run -d -p 8080:80 hub.c.163.com/library/nginx:1.13.0
```

以上命令标识将$docker$容器的80端口映射到宿主机的8080端口。如果使用`docker ps`命令查看，也可以看到端口映射规则

![image-20210920192808358](https://gitee.com/cao_ziqiang/img/raw/master/20210920192808.png)

有时一个容器可能不止需要暴露一个端口时，此时可以使用大写$P$由$docker$指定映射规则。

```dockerfile
docker run -d -P hub.c.163.com/library/nginx:1.13.0
```

一般会是32768端口。

### $host$

和宿主机使用一样的网络，自动映射到宿主机的端口。

### $none$

没有网络

## 制作$Docker$容器

如制作以下容器

首先创建$Dockerfile$文件

```dockerfile
FROM alpine:latest
MAINTAINER homyit
CMD echo "hello my dockerfile"
```

![image-20210920195454421](https://gitee.com/cao_ziqiang/img/raw/master/20210920195454.png)

前面不小心还敲错了几次

```dockerfile
docker build -t hello_docker .
```

运行自己的$docker$容器

![image-20210920195652737](https://gitee.com/cao_ziqiang/img/raw/master/20210920195652.png)

