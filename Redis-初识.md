---
title: Redis-初识
categories:
  - Redis
tags:
  - backend
  - Redis
abbrlink: 2cef2a6c
date: 2021-08-28 10:56:35
---

## Redis介绍

- Key-Value型的NoSQL数据库;
- 将数据存储在内存中,也可以**持久化**到磁盘;
- 常用于缓存,利用内存的高效提高数据的读写速度;

### 特点

- 速度快,读写峰值可以达到10W;
- 广泛语言支持,对各种服务端语言都可以支持;
- 可以持久化;
- 支持多种数据结构;
- 支持主从复制,搭建分布式环境;

- 高并发高可用,支持哨兵Sentinel机制;

### 安装

linux下安装:

1. 首先安装gcc,redis是使用C语言编写的,所以下载下源码后需要用gcc编译

```bash
yum install gcc
```

2. 安装好gcc后,再创建redis目录,下载redis源码包;

```bash
wget http://download.redis.io/releases/redis-5.0.2.tar.gz
```

3. 解压缩

```bash
tar -xvf redis-5.0.2.tar.gz
```

4. 进入解压目录,编译

```bash
make
```

当出现

<pre>
Hint: It's a good idea to run 'make test' ;)
</pre>

表示编译完成;

5. 启动

```bash
./src/redis-server redis.conf
```

出现redis的启动动画就表示安装完成了.

![image-20210828164907070](https://gitee.com/cao_ziqiang/img/raw/master/20210828164907.png)

### 配置

1. 设置为守护进程模式

编辑`redis.conf`,把`daemonize`修改为`yes`；

2. 修改端口号,生产环境下使用默认端口容易遭受黑客攻击等;

3. 记录日志,`logFIle` `"redis.log"`
4. 修改数据库数量,databases修改为想要的数字;
5. 设置密码,修改`requirepass`后跟密码;
6. 数据目录,在dir后修改数据存放目录;

### 命令

select	 选择数据库;

set		   设置key-value;

get		   获得key的value;

keys	    查询符合条件的key;

dbsize	返回key的总数;

exists	  检查key是否存在;

del		   删除key的数据;

expire	 设置key的有效期;

ttl			 查看过期剩余时间;

persist    去掉key的过期时间;

type		查看key的类型;

### 数据结构和内部编码

![image-20210828232619223](https://gitee.com/cao_ziqiang/img/raw/master/20210828232619.png)

在redis内部定义了redisObject结构体

```c
struct redisObject{
	// 对外的数据类型(type)
	// 编码方式(encoding)
	// 数据指针(ptr)
	// 虚拟内存(vm)
	// 其他信息
}
```

### 单线程

![image-20210828232927814](https://gitee.com/cao_ziqiang/img/raw/master/20210828232927.png)

redis是串行化执行命令,一次只能有一条语句被执行;

并且redis拒绝长(慢)命令

<pre>
keys,flushall,flushdb,slow lua script,
mutil/exec,operate big value(coolection)
</pre>



redis单线程速度快的原因:

1. 纯内存;
2. 非阻塞I/O,使用epoll模型;
3. 单线程避免了线程切换和竞态消耗;

![image-20210828233246150](https://gitee.com/cao_ziqiang/img/raw/master/20210828233246.png)

也有说法认为redis并不完全是单线程,在进行文件读写时

```c
fysnc file descriptor
close file descriptor
```

