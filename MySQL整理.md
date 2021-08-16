---
title: MySQL整理
date: 2021-08-15 22:22:48
categories: MySQL
tags: [SQL,backend,MySQL]
---

## 一、数据库简介

### 1.1 简介

**数据库是数据存放的载体。**常见的操作系统Windows、Linux、MacO都是基于文件的操作系统。

**但是如果直接使用文件来保存数据后，不方便提取数据。**

这时我们需要使用数据库系统来将数据进行存储，数据库为我们屏蔽将数据存入操作系统的底层的操作，提供一套数据库操作语言来方便数据的读写。

### 1.2 分类

关系型数据库（rdbms)

即使用了关系模型的数据库系统，在关系模型中，数据按类别存放，数据之间有联系。

主流关系型数据库：

IBM		 ->	DB2数据库（性能好，采购成本非常高，用于电信、金融领域）

Oracle	->	Oracle数据库（性能好，采购成本也较高，按照CPU个数收费，但不适用于分布式集群）

Oracle	->	MySQL数据库（性能不及前面二者，但是开源免费，可以进行二次开发）

MircoSoft	->	SQL Server数据库（图形界面好，教育行业免费，早期不支持跨平台，生态不好）

NoSQL数据库

数据分类存放，但是数据之间没有关联关系

Redis	->	使用内存来保存数据的一种key-value数据库

MemCache	->	和Redis类似，也使用内存，份额不如Redis大

MongoDB	->	使用硬盘存储数据的文档数据库，适用于海量低价值数据

Neo4j	->	使用内存保存数据的图数据库，适用于保存关系

## 二、MySQL

### 2.1 简介

MySQL是应用最广泛，普及度最高的开源关系型数据库。

MySQL有许多发行版本，比较著名的有Oracle官方的MySQL,Percona的PERCONA（性能优秀，但是只支持Linux系统）,MariaDB（由MySQL发明人创立的基金会开源）

### 2.2 SQL语言

SQL是用于访问和处理数据的标准计算机语言，相当于数据库系统留给开发者的接口。

SQL语句可分为DML(添加、修改、删除、查询)、DCL(用户、权限、事务)、DDL(逻辑库、数据表、视图、索引)

## 三、 MySQL方言

每个数据库系统的SQL语言和标准的SQL有一定的区别，称之为方言。

### 3.1 创建逻辑表

```mysql
# 创建逻辑库
CREATE DATABASE 逻辑库名称;
# 查看逻辑库
SHOW DATABASES;
# 删除逻辑库
DROP DATABASE 逻辑库名称；
# 使用逻辑库
USE 逻辑库名称;
```

### 3.2 创建数据库

```mysql
CREATE TABLE 数据表(
	列名1 数据类型 [约束] [COMMENT 注释],
	列名2 数据类型 [约束] [COMMENT 注释],
	...
)[COMMENT = 注释];
```

如下：

```mysql
CREATE TABLE `student` (
  `id` int(10) unsigned NOT NULL,
  `name` varchar(20) NOT NULL,
  `sex` char(1) NOT NULL,
  `birthday` date NOT NULL,
  `tel` char(11) NOT NULL,
  `remark` varchar(200) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

### 3.3 数据类型

数字

| 类型      | 大小  | 说明         |
| --------- | ----- | ------------ |
| TINYINT   | 1字节 | 小整数       |
| SMALLINT  | 2     | 普通整数     |
| MEDIUMINT | 3     | 普通整数     |
| INT       | 4     | 较大整数     |
| BIGINT    | 8     | 大整数       |
| FLOAT     | 4     | 单精度浮点数 |
| DOUBLE    | 8     | 双精度浮点数 |
| DECIMAL   | ----  | 精确小数     |

字符串

| 类型       | 大小           | 说明             |
| ---------- | -------------- | ---------------- |
| CHAR       | 1-255字符      | 固定长度字符串   |
| VARCHAR    | 1-65535字符    | 不固定长度字符串 |
| TEXT       | 1-65535字符    | 不确定长度       |
| MEDIUMTEXT | 1-1千6百万字符 | 不确定长度       |
| LONGTEXT   | 1-42字符       | 不确定长度       |

日期类型

| 类型      | 大小  | 说明                                 |
| --------- | ----- | ------------------------------------ |
| DATE      | 3字节 | 日期,特殊的字符串，年月日之间用-分隔 |
| TIME      | 3字节 | 时间，二十四小时制时间               |
| YEAR      | 1字节 | 年份                                 |
| DATETIME  | 8字节 | 日期时间                             |
| TIMESTAMP | 4字节 | 时间戳，只能是1970年之后的时间       |

### 3.4 修改表结构

添加字段

```mysql
ALTER TABLE 表名称
	ADD 列1 数据类型 [约束] [COMMENT 注释],
	ADD 列2 数据类型 [约束] [COMMENT 注释],
	...
;
```

修改字段和约束

```mysql
ALTER TABLE 表名称
	MODIFY 列1 数据类型 [约束] [COMMENT 注释],
	MODIFY 列2 数据类型 [约束] [COMMENT 注释],
	...
;
```

修改字段名称

```mysql
ALTER TABLE 表名称
	CHANGE 列1 新列名1 数据类型 [约束] [COMMENT 注释],
	CHANGE 列2 新列名2 数据类型 [约束] [COMMENT 注释],
	...
;
```

删除字段

```mysql
ALTER TABLE 表名称
	DROP 列1,
	DROP 列2,
	...
;
```

修改字段排序

```mysql
ALTER TABLE 表名称
	ADD 列名 数据类型 [约束] AFTER 列名,
;
```

表名修改

```mysql
ALTER TABLE 表名称 RENAME 新表名;
```



### 3.5 字段约束

#### 3.5.1 三大范式：

第一范式：原子性，这一点是关系型数据库的基本要求，不满足这一点就不是关系型数据库

**数据表的每一列都是不可分割的基本数据项，同一列中不能有多个值，也不能存在重复的属性。**

| 学号 | 姓名 | 班级          |
| ---- | ---- | ------------- |
| 1001 | 张三 | 2019级计科1班 |

上述记录不满足第一范式，因为班级还可以再分为年级和班级两个字段

| 学号 | 姓名 | 年级   | 班级    |
| ---- | ---- | ------ | ------- |
| 1001 | 张三 | 2019级 | 计科1班 |

第二范式：唯一性。

数据表中的每条记录必须是唯一的。为了实现区分，通常要为表加上一个列用来存储唯一标识，这个唯一属性

列被称作主键列

| 学号 | 成绩 | 日期       |
| ---- | ---- | ---------- |
| 1001 | 90   | 2021-08-12 |
| 1001 | 80   | 2021-08-13 |

无法区分重复的数据

| 流水号   | 学号 | 成绩 | 日期       |
| -------- | ---- | ---- | ---------- |
| 20210812 | 1001 | 90   | 2021-08-12 |
| 20210813 | 1001 | 80   | 2021-08-13 |

数据具有唯一性

第三范式：关联性

每列都与主键有直接关系，不存在传递依赖，若满足了第三范式，则说明也满足了第一、第二范式

| 爸爸 | 儿子 | 女儿 | 女儿的玩具 | 女儿的衣服 |
| ---- | ---- | ---- | ---------- | ---------- |
| 陈华 | 陈浩 | 陈婷 | 海绵宝宝   | 校服       |

以爸爸列作为主键列，此时女儿的玩具和女儿的衣服和主键没有关系，违反了第三范式

这时需要将表进行拆分

| 爸爸 | 儿子 | 女儿 |
| ---- | ---- | ---- |
| 陈华 | 陈浩 | 陈婷 |



| 女儿 | 女儿的玩具 | 女儿的衣服 |
| ---- | ---------- | ---------- |
| 陈婷 | 海绵宝宝   | 校服       |

不满足范式会导致数据存放松散，检索效率低下

依照第三范式，数据可以拆分保存到不同的数据表，彼此保持关联

| 约束名称 | 关键字      | 描述                               |
| -------- | ----------- | ---------------------------------- |
| 主键约束 | PRIMARY KEY | 字段值唯一，且不能为NULL           |
| 非空约束 | NOT NULL    | 字段值不能为NULL                   |
| 唯一约束 | UNIQUE      | 字段值唯一，且可以为NULL           |
| 外键约束 | FOREIGN KEY | 保持关联数据的逻辑性（不推荐使用） |

#### 3.5.2主键约束

- 主键约束要求字段的值在全表必须唯一，而且不能为NULL值
- 建议主键—定要使用数字类型，因为数字的检索速度会非常快
- 如果主键是数字类型，还可以设置自动增长

```mysql
CREATE TABLE t_teacher (
	id INT PRIMARY KEY AUTO_INCREMENT,
	...
) ;
```

#### 3.5.3 非空约束

- 非空约束要求字段的值不能为NULL值

- NULL值以为没有值，而不是“”空字符串

```mysql
	CREATE TABLEt teacher (
		id INT PRIMARY KEY AUTO_INCREMENT,
		name VARCHAR(200)NOT NULL,
		married BOOLEAN NOT NULL DEFAULT FALSE
	) ;
```

#### 3.5.4 唯一约束

- 唯—约束要求字段值如果不为NULL，那么在全表必须唯一

```mysql
	CREATE TABLE t_teacher (
	tel CHAR (11) NOT NULL UNIQUE
	) ;
```

#### 3.5.5 外键约束

- 外键约束用来保证关联数据的逻辑关系
- 外键约束的定义是写在子表上的

父表

```mysql
CREATE TABLE t_dept (
	deptno INT UNSIGNED PRIMARY KEY,
	dname VARCHAR (20)NOT NULL UNIQUE,
	tel CHAR(4) UNIQUE
) ;
```

子表

```mysql
CREATE TABLE t_emp (
	empno INT NSIGNED PRIMARY KEY,
    ename VARCHAR (20)NOT NULL,
	sex ENUM( "男","女")NOT NULL,deptno INT UNSIGNED,
	hiredate DATE NOT NULL,
	FOREIGN KEY (deptno) REFERENCES t_dept (deptno)
);
```

- 如果形成外键闭环，我们将无法删除任何一张表的记录,所以尽量不适用外键约束

### 3.6 索引

用于提升数据检索的速度。机制就是基于排序，一旦数据排序之后，查找的速度就会翻倍，现实世界跟程序世界都是如此。

例如，我们想查找机会opportunity这个单词，但是我们只记得开头是op，我们拿到了一本英文词典就需要先快速定位到o开头的区间，小范围查找opportunity这个单词。否则就需要一页一页的查找这个单词，效率无疑是很低的。

#### 3.6.1 创建索引

```mysql
CREATE TABLE 表名称(
	......,
	INDEX [索引名称] (字段),
	......
);
```

设置好索引好，数据库就会对数据字段进行排序，生成二叉树

#### 3.6.2 添加索引和删除索引

```mysql
# 添加索引
CREATE  INDEX 索引名称 ON 表名(字段);
ALTER TABLE 表名称 ADD INDEX [索引名称](字段);
# 查看索引
SHOW INDEX FROM 表名;
# 删除索引
DROP INDEX 索引名称 ON 表名;
```

#### 3.6.3 索引使用原则

数据量很大，而且经常被查询的数据表可以设置索引；

索引只添加在经常被用作检索条件的字段上面；

不要在大字段上创建索引；

