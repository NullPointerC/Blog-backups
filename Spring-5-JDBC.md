---
title: Spring-5-JDBC
date: 2021-09-04 13:52:40
categories: FrameWork
tags: [FrameWork,backend,Spring,Java]

---

## Spring JDBC

Spring JDBC是Spring框架用于处理关系型数据库的模块；

Spring JDBC对JDBC API进行封装，极大简化开发工作量；

JdbcTemplate是Spring JDBC核心类，提供数据CRUD方法；

比起mybatis是一种轻量级的封装，执行效率和速度上更优于mybatis；



## CURD API

### 查询

queryForObject；单条记录查询

query；多条记录查询

方法签名都是sql，参数，以及转换对象BeanPropertyRowMapper。

queryForList；将每条记录用Map封装，key是字段名，value是字段值，并用list封装；

### 修改、删除、写入

update泛指所有DML语句；

方法形参为SQL语句和参数，返回值为数据库影响行数；

## 事务

### 编程式事务

通过代码手动提交回滚事务的事务控制方法；

Spring JDBC提供了TransactionManager事务控制器实现事务的管理，事务管理器提供commit、rollback方法进行提交、回滚操作；

编程式事务优点在于事务的开启、提交、回滚全部由程序员控制，可控程度较高，但是对开发不太友好，可能会存在大量重复的代码用来开启、提交、回滚事务等。在一些对安全性能有要求的领域更适合。

### 声明式事务

声明式事务指在不修改源码情况下通过配置形式自动实现事务控制,声明式事务本质就是AOP环绕通知。

当目标方法执行成功时,自动提交事务；

当目标方法抛出运行时异常时,自动事务回滚；

### 事务传播方式

事务传播行为是指多个拥有事务的方法在嵌套调用时的事务控制方式；

xml形式：

```xml
<tx:method name="..." propagation="REQUIRED">
```

注解形式：

```java
@Transactional(propagation=Propagation.REQUIRED)
```

![image-20210904145407749](https://gitee.com/cao_ziqiang/img/raw/master/20210904145407.png)

### 注解配置

只需要加上@Transactional注解，并且可以配置事务传播行为，