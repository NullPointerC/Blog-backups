---
title: Redis-数据类型
categories:
  - Redis
tags:
  - backend
  - Redis
abbrlink: b4681a1
date: 2021-08-28 17:30:10
---

## 数据类型

### String-字符串

绝大多数都是字符串,很多key对应的value都是字符串类型;

String最大容纳512mb,建议单个字符串存储不超过100k;

相比起set/get,还可以使用mset/mget一次性设置多个key-value;

incr/decr,使key值自增或自减.incrby/decrby使key自增或自减指定步长;

### Hash-键值

用于存储结构化数据,如实体对象等

hget/hset获取或者设置hash中的key;

hmset、hmget、hgetall对多个值进行操作;

hdel删除hash;

hexists检查hash是否存在;

hlen获取指定长度; 

### List-列表

list列表就是一系列字符串的数组,按插入的先后顺序排序;

最大长度为2^32 - 1,最大可包含40亿个元素;

lpush、rpush在左侧或右侧添加元素;

lpop、rpop左侧、右侧弹出;

lrange输出list指定范围的元素;

### Set-集合

无序,成员唯一;

### Zset-有序集合

有序,成员唯一,按添加顺序排序;

<hr/>

在redis后续版本添加,本质还是string

### BitMap-位图

布隆过滤器等;

### HyperLogLog

超小内存唯一值计数

### GEO

地理信息定位

## Jedis

Jedis是Java对redis的操作工具包;

引入依赖

```xml
<dependency>
	<groupId>redis.clients</groupId>
	<artifactId>jedis</artifactId>
	<version>2.9.0</version>
</dependency>
```

在Jedis中,方法和redis-cli中的命令名称一致,参数也一致;

## 应用场景

### 缓存系统

用户的请求来到服务器,先查询缓存中是否有,减小DB的压力;

![image-20210828225728944](http://static.codenote.xyz/img/20210828225729.png)

### 计数器

如微博中的点赞数,转发数,因为redis提供了incr、decr等操作，可以在单线程下保证并发性地修改数据而不会出错;

![image-20210828230022145](http://static.codenote.xyz/img/20210828230022.png)

### 消息队列系统

![image-20210828230109898](http://static.codenote.xyz/img/20210828230110.png)

### 排行榜

redis提供了Zset有序集合,十分符合排行榜的设定

![image-20210828230207634](http://static.codenote.xyz/img/20210828230207.png)

### 社交网络

![image-20210828230241369](http://static.codenote.xyz/img/20210828230241.png)

很多社交网络的功能也会使用redis来实现,比如粉丝数,关注数,共同关注,时间轴等;

### 实时系统

如布隆过滤器,对于垃圾信息的过滤;

![image-20210828230401659](http://static.codenote.xyz/img/20210828230401.png)



