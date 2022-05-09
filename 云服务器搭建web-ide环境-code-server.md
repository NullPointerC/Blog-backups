---
title: '云服务器搭建web-ide环境[code-server]'
categories: Docker
tags: 'WebIde, Docker'
abbrlink: bfb78de6
date: 2022-04-16 20:41:01
---

今天在逛阿里云的时候，发现了服务器的一个新用途。

![image-20220416204219657](https://static.codenote.xyz/img/202204162042794.png)

一开始想着是以为可以无缝迁移，但是后来发现需要重置现有的环境，但是目前我的服务器上已经跑了许多服务，备份镜像和迁移数据是一份很大的工作量，并且目前我目前闲置的这台服务器上面跑了3个网站，5个docker的容器，还有redis，mysql，mq都在这上面，如果重置了就很不够geek😂。so，又去搜索了类似的web-ide产品，发现还是有很多种web-ide的，[link](https://www.infoq.cn/article/2018%2F07%2Fwebide-explorecode)。

这里我对比了几款产品，最终剩下了coder公司的「[code-server](https://github.com/coder/code-server)」和eclipse基金会的「[THEIA](https://github.com/eclipse-theia/theia)」，首先因为都可以运行在docker里，对于我这种喜欢物理隔离的用户来说极度友好。

后面又比对了一番，还是决定先用coder-server，因为插件比较多，而且出来的时间比THEIA久一点，更稳一些。

这里的[doc](https://coder.com/docs/code-server/latest)也挺详细的，但是就是有一点不好，全英压力有点大。。。个人也是捣鼓了一下午才弄好，其中的坑点还是比较多的，这里记录一下。

这里官网给出了几种安装方式，可以选择本地部署也可以选择docker部署，前面也提到个人有系统环境洁癖，不同的环境之间造成污染和版本不一致问题可太难受了，所以就使用了docker的方式部署。

![image-20220416210624106](https://static.codenote.xyz/img/202204162106212.png)

首先需要确保你的服务器上有docker，如果没有，自行google😂，how to install docker in your os即可。

然后再去把镜像拉下来

```dockerfile
docker pull codercom/code-server:latest
```

拉下来之后再开始跑容器即可，这里需要注意的是，code-server需要一个8080端口的映射，文件挂载这个问题其实可以根据需要自行选择，这里我为了方便改密码和配置信息，同时将项目挂载到宿主机上，所以选择挂载了两个文件。

因为我这里需要挂载文件，所以先行创建两个文件夹。

```sh
mkdir /root/.config
mkdir -p /usr/local/coder/project
cd /usr/local/coder/project
```

然后再启动容器

```docker
docker run -it --name code-server -p 8080:8080 \
  -v "$HOME/.config:/root/.config" \
  -v "$PWD:/root/project" \
  -u "$(id -u):$(id -g)" \
  -e "DOCKER_USER=$USER" \
  codercom/code-server:latest
```

这里我的命令的含义是将容器的端口还是按源端口映射，并挂载两个文件，设置docker内用户为当前的用户。

然后运行成功后，第一次访问需要的密码是随机初始化的，所以需要查看密码。

![image-20220416211831470](https://static.codenote.xyz/img/202204162118513.png)

这里的密码一般初始都是随机的，可以先行调整后再重启。

然后就可以先测试是否能够正常访问，因为这里配置的是8080端口，所以将服务器的8080端口和防火墙暂时开启，访问your ip:8080即可。

如果显示如下：

![image-20220416212111072](https://static.codenote.xyz/img/202204162121168.png)

则说明容器启动正常，这时再输入前一步的密码即可。

输入后，进入ide后的界面如普通vs-code所示：

![image-20220416212220046](https://static.codenote.xyz/img/202204162122147.png)

这里完成部署后，即可将8080端口关闭了，因为我们需要将8080通过nginx代理出去，并且添加https。

这里有个坑点，因为需要用nginx代理WebScoket协议，所以再header部分需要做一些额外配置信息，否则会一直报一个错误让页面reload。

配置文件贴在下方：

![image-20220416213055546](https://static.codenote.xyz/img/202204162130651.png)

主要就是location部分中的`proxy_set_header`部分。

然后正常访问[link](https://ide.codenote.xyz)即可，再输入自定义的密码，即可进入ide了。![image-20220416213256583](https://static.codenote.xyz/img/202204162132700.png)

其他用法都和vscode很类似，(●'◡'●)。