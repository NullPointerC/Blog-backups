---
title: Mybatis使用小结
categories:
  - FrameWork
tags:
  - FrameWork
  - backend
  - Mybatis
abbrlink: 571c4c62
date: 2021-08-19 11:12:41
---

## 一、框架简述

框架是可被应用开发者定制的应用骨架

SSM框架	->	`Spring+SpringMVC+Mybatis`

## 二、Mybatis

持久层技术框架,使用XML使SQL与程序解耦

### 2.1 环境配置

MyBatis采用XML格式配置数据库环境信息

MyBaits环境配置标签`<environment>`

environment包含数据库驱动、URL、用户名与密码

### 2.2 核心对象

`SqlSessionFactory`是MyBatis的核心对象,用于初始化MyBatis,创建`SqISession`对象,保证应用中全局唯一;

`SqlSessionFactory` 的实例可以通过 `SqlSessionFactoryBuilder` 获得。

而 `SqlSessionFactoryBuilder` 则可以从 XML 配置文件或一个预先配置的 Configuration 实例来构建出 SqlSessionFactory 实例。

XML 配置文件中包含了对 MyBatis 系统的核心设置，包括获取数据库连接实例的数据源（DataSource）以及决定事务作用域和控制方式的事务管理器（TransactionManager）。

SqlSession是MyBatis操作数据库的核心对象，SqlSession使用JDBC方式与数据库交互，SqlSession对象提供了数据表CRUD对应方法

### 2.3 数据查询流程

1. 创建实体类(entity)
2. 创建Mapper XML
3. 编写`<select>`标签
4. 开启驼峰命名映射
5. 新增`<mapper>`标签
6. SqlSession执行select语句

select 元素允许配置很多属性来配置每条语句的行为细节

[详情link](https://mybatis.net.cn/sqlmap-xml.html#select)

### 2.4 结果映射

ResultMap可以将查询结果映射为复杂类型的Java对象,适用于Java对象保存多表关联结果,支持对象关联查询等高级特性

`<resultMap>`标签中，property代表实体的属性，column代表查询结果中的列名

### 2.5 数据增、删、改

Insert, Update, Delete 元素的属性

[详情link](https://mybatis.net.cn/sqlmap-xml.html#select)

selectKey标签需要明确编写获取最新主键的SQL语句

useGeneratedKeys属性会自动根据驱动生成对应SQL语句

[详情link](https://mybatis.net.cn/sqlmap-xml.html#select)

## 三、高级特性

### 3.1 日志管理

用于记录系统操作记录，保存历史记录，是诊断系统的重要依据

[参考](https://www.codenote.xyz/java/2021/05/18/springboot-ri-zhi/)

可以通过在 MyBatis 配置文件 mybatis-config.xml 里面添加一项 setting 来选择其它日志实现。



### 3.2 动态SQL

动态SQL是指根据参数数据动态组织SQL的技术

- if
- choose (when, otherwise)
- trim (where, set)
- foreach

### 3.3 缓存

面对重复查询可以开启缓存，这样可以减少MySQL的检索次数；

—级缓存默认开启,缓存范围SqlSession会话；

二级缓存手动开启,属于范围Mapper Namespace；

![image-20210820161814713](http://static.codenote.xyz/img/20210820161814.png)

缓存规则：

1. 二级开启后默认所有查询操作均使用缓存；
2. 写操作commit提交时对该namespace缓存强制清空；
3. 配置useCache=false可以不用缓存
4. 配置flushCache=true代表强制清空缓存

要启用全局的二级缓存，只需要在你的 SQL 映射文件中添加一行：

`<cache>`

[详情link](https://mybatis.net.cn/sqlmap-xml.html#cache)

可以做如下配置：

eviction：

- `LRU` – 最近最少使用：移除最长时间不被使用的对象。
- `FIFO` – 先进先出：按对象进入缓存的顺序来移除它们。
- `SOFT` – 软引用：基于垃圾回收器状态和软引用规则移除对象。
- `WEAK` – 弱引用：更积极地基于垃圾收集器状态和弱引用规则移除对象。

默认的清除策略是 LRU。

flushInterval（刷新间隔）属性可以被设置为任意的正整数，设置的值应该是一个以毫秒为单位的合理时间量。 默认情况是不设置，也就是没有刷新间隔，缓存仅仅会在调用语句时刷新。

size（引用数目）属性可以被设置为任意正整数，要注意欲缓存对象的大小和运行环境中可用的内存资源。默认值是 1024。

readOnly（只读）属性可以被设置为 true 或 false。只读的缓存会给所有调用者返回缓存对象的相同实例。 因此这些对象不能被修改。这就提供了可观的性能提升。而可读写的缓存会（通过序列化）返回缓存对象的拷贝。 速度上会慢一些，但是更安全，因此默认值是 false。

二级缓存是事务性的。这意味着，当 SqlSession 完成并提交时，或是完成并回滚，但没有执行 flushCache=true 的 insert/delete/update 语句时，缓存会获得更新。

使用自定义缓存

[详情link](https://mybatis.net.cn/sqlmap-xml.html#cache)

### 3.4 关联查询

#### OneToMany

在Java对象模型中，需要在One的一方持有Many一方的集合，Many的一方需要持有One的一方的标识；

在xml中，需要对One一方到Many的结果映射配置`<resultMap>`标签以及`<collection>`标签，后者代表多的一方的集合；

在Many的一方需要配置对One的一方标识的查询语句；

#### ManyToOne

在Java对象模型中，需要在One的一方持有Many一方的集合，Many的一方需要持有One的一方的标识以及One的一方的实体；

在xml中，需要在Many中配置`<resultMap>`作为Many方的实体，并配置`<association>`映射到One的一方实体关联映射；

在One的一方中，需要编写对Many中持有One的实体标识查询语句；

### 3.5 PageHelper

引入maven依赖以及mybatis配置文件中的<plugins>配置

[官方文档](https://pagehelper.github.io/)

### 3.6 连接池

引入连接池的jar包，并编写连接池工厂继承自`UnpooledDataSourceFactory`

### 3.7 批处理

在`<insert>`或`<delete>`标签中加入`<foreach>`和`<collection>`

## 四、注解开发

[文档link](https://mybatis.net.cn/java-api.html)