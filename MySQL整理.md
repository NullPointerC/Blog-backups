---
title: MySQL整理
categories:
  - MySQL
tags:
  - backend
  - MySQL
abbrlink: bf2339a8
date: 2021-08-15 22:22:48
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

## 四、查询

### 4.1 单表查询

最基本的查询语句是由SELECT 和FROM关键字组成的；

SELECT语句屏蔽了物理层的操作，用户不必关心数据的真实存储，交由数据

库高效的查找数据；

通常情况下，SELECT子句中使用了表达式，那么这列的名字就默认为表达

式，因此需要一种对列名重命名的机制-AS关键字；

### 4.2 查询子句的执行顺序

1. 词法分析与优化读取SQL语句；
2. FROM+JOIN，选择数据来源；
3. WHERE, 筛选数据分类；
4. GROUP BY，将数据分组；
5. HAVING，对分组的数据进行筛选；
6. SELECT 选择输出内容；
7. ORDER BY 对数据进行排序；
8. LIMIT 对数据进行分页；

### 4.3 数据分页

比如我们查看朋友圈，只会加载少量部分信息，不用一次性加载全部朋友圈，那样只会浪费CPU时间、内存和

网络带宽，所以查询时可以对数据进行限定，限定结果集的数量

```mysql
SELECT
列名
FROM
表名
LIMIT 起始位置，偏移量;
```

如果LIMIT子句只有一个参数，它表示的是偏移量，起始值默认为O

### 4.4 结果集排序

如果没有设置，查询语句不会对结果集进行排序。也就是说,如果想让结果集按照某种顺序排列，就必须使用`ORDER BY`子句。

```mysql
SELECT
列名
FROM
表名
ORDER BY 列名[ASC|DESC];
```

`ASC`代表升序(默认)，`DESC`代表降序

如果排序列是数字类型，数据库就按照数字大小排序，如果是日期类型就按照日期大小排序，如果是字符串就按照字符集序号排序。

我们可以使用`ORDER BY`规定首要排序条件和次要排序条件。数据库会先按照首要排序条件排序，如果遇到首要排序内容相同的记录，那么就会启用次要排序条件接着排序。

### 4.5 结果集去重

如果我们需要去除重复的数据，可以使用DISTINCT关键字来实现

```mysql
SELECT 
DISTINCT 字段 
FROM
表名;
```

使用`DISTINCT`的`SELECT`子句中只能查询一列数据，如果查询多列,去除重复记录就会失效。

`DISTINCT`关键字只能在`SELECT`子句中使用一次

### 4.6 条件查询

很多时候，用户感兴趣的并不是逻辑表里的全部记录，而只是它们当中能够满足某一种或某几种条件的记录。这类条件要用WHERE子句来实现数据的筛选

```mysql
SELECT 
...... 
FROM 
......
WHERE 条件[AND| OR]条件
;
```

WHERE子句中，条件执行的顺序是从左到右的。所以我们应该把索引条件，或者筛选掉记录最多的条件写在最左侧;

## 五、数据统计

### 5.1 聚合函数

聚合函数在数据的查询分析中，应用十分广泛。聚合函数可以对数据求和、求最大值和最小值、求平均值等等。

SUM函数

SUM函数用于求和，只能用于数字类型，字符类型的统计结果为O，日期类型统计结果是毫秒数相加。

MAX函数

MAX函数用于获得非空值的最大值。

MIN函数

MIN函数用于获得非空值的最小值。

AVG函数

AVG函数用于获得非空值的平均值，非数字数据统计结果为0

COUNT函数

COUNT(*)用于获得包含空值的记录数，COUNT(列名)用于获得包含非空值的记录数。

**注意聚合函数不能用在WHERE子句的条件中**

### 5.2 分组查询

默认情况下汇总函数是对全表范围内的数据做统计

GROUP BY子句的作用是通过一定的规则将一个数据集划分成若干个小的区域，然后针对每个小区域分别进行数据汇总处理

逐级分组

数据库支持多列分组条件，执行的时候逐级分组。

查询语句中如果含有GROUP BY子句，那么SELECT子句中的内容就必须要遵守规定:SELECT子句中可以包括聚合函数，或者GROUPBY子句的分组列，其余内容均不可以出现在SELECT子句中

对分组结果集再次做汇总计算

WITH ROLLUP关键字对聚合后的数据再做汇总

GROUP CONCAT函数

GROUP_CONCAT函数可以把分组查询中的某个字段拼接成一个字符串

### 5.3 HAVING 子句

HAVING子句为了解决WHERE子句不知道查询范围的问题而出现的。

```mysql
SELECT deptno
FROM t_emp
WHERE avg(sal)>=2000
GROUP BY deptno;
```

如上查询，执行将会报错，因为WHERE子句优先级高于GROUP子句，但是WHERE不知道应该对谁进行筛选。

这是就需要限定分组的范围就用到了HAVING子句，HAVING子句紧跟着GROUP BY子句

```mysql
SELECT deptno
FROM t_emp
GROUP BY deptno HAVING avg(sal)>=2000;
```

HAVING子句的特殊用法

按照数字1分组，MySQL会依据SELECT子句中的列进行分组HAVING子句也可以正常使用

### 5.4 表连接

从多张表中提取数据，必须指定关联的条件。如果不定义关联条件就会出现无条件连接，两张表的数据会交叉连接，产生笛卡尔积。

规定了连接条件的表连接语甸，就不会出现笛卡尔积。

表连接分为两种:内连接和外连接

内连接是结果集中只保留符合连接条件的记录

外连接是不管符不符合连接条件，记录都要保留在结果集中

#### 5.4.1 内连接

内连接是最常见的一种表连接，用于查询多张关系表符合连接条件的记录。

```mysql
SELECT ......
FROM 表1
[INNER] JOIN 表2 ON 条件
[INNER] JOIN 表3 ON 条件
......
```

内连接有多种表达形式

```mysql
SELECT ...... FROM 表1 JOIN 表2 ON 连接条件;
SELECT ...... FROM 表1 JOIN 表2 WHERE 连接条件
SELECT ...... FROM 表1，表2 WHERE 连接条件;
```

内连接的数据表不一定必须有同名字段，只要字段之间符合逻辑关系就可以

相同的数据表也可以做表连接

#### 5.4.2 外连接

外连接与内连接的区别在于，除了符合条件的记录之外，结果集中还会保留不符合条件的记录。

左外连接就是保留左表所有的记录，与右表做连接。如过有符合条件的记录就与左表连接。如果右表没有

符合条件的记录,就用NULL与左表连接。右外连接也是如此。

UNION关键字可以将多个查询语句的结果集进行合并。

内连接只保留符合条件的记录，所以查询条件写在ON子句和WHERE子句中的效果是相同的。但是外连接里，

条件写在WHERE子句里，不合符条件的记录是会被过滤掉的，而不是保留下来。

### 5.5 子查询

子查询是一种查询中嵌套查询的语句，但是不推荐使用WHERE子句中，更推荐使用FROM子句中。

#### 5.5.1 WHERE子查询

这种子查询最简单，最容易理解，但是却是效率很低的子查询

如：查询底薪超过公司平均底薪的员工的信息

```mysql
SELECT 
empno,ename,sal
FROM t_emp
WHERE sal >=(SELECT AVG(sal) FROM t_emp);
```

如上查询在比较每条记录时都要重新执行子查询。

WHERE子句中，可以使用IN、ALL、ANY、EXISTS关键字来处理多行表达式结果集的条件判断

EXISTS关键字是把原来在子查询之外的条件判断，写到了子查询的里面。

#### 5.5.2 FROM子查询

这种子查询只会执行一次，所以查询效率很高

如上案例如果使用FROM子查询可以写成

```mysql
SELECT 
	e.empno,e.ename,e.sal
FROM t_emp e JOIN
	(SELECT deptno, AVG(sal) as avg
	FROM t_emp GROUP BY deptno) t
ON e.deptno=t.deptno and e.sal>=t.avg;
```

这种查询只会执行一次，所以查询效率很高

#### 5.5.3 SELECT子查询

这种子查询每输出一条记录的时候都要执行一次，查询效率很低

```mysql
SELECT 
	e.empno,
	e.ename,
	(SELECT dname FROM t_dept WHERE deptno = e.deptno)
FROM t_emp e;
```



## 六、DML语句

### 6.1 INSERT 语句

INSERT语句可以向数据表写入记录，可以是一条记录，也可以是多条记录

```mysql
INSERT INTO 表名
(字段1,字段2,...)
VALUES(值1,值2,...);
```

MySQL也支持插入多条数据

```mysql
INSERT INTO 表名
(字段1,字段2,...)
VALUES(值1,值2,...),(值1,值2,...);
```

INSERT语句方言

```mysql
INSERT [INTO] 表名
SET 字段1=值1,字段2=值2,...;
```

IGNORE关键字会让INSERT只插入数据库不存在的记录

```mysql
INSERT [IGNORE] INTO 表名
...;
```

### 6.2 UPDATE语句

UPDATE语句用于修改表的记录

```mysql
UPDATE [IGNORE] 表名
SET 字段1=值1,字段2=值2,...
[WHERE 条件1 ...]
[ORDER BY ...]
[LIMIT ...];
```

因为相关子查询效率非常低，所以我们可以利用表连接的方式来改造UPDATE语句

```mysql
UPDATE 表1 JOIN 表2 ON 条件
SET 字段1 = 值1,字段2 = 值2,...;
```

表连接的UPDATE语句可以修改多张表的数据

UPDATE语句的表连接既可以是内连接，又可以是外连接

```mysql
UPDATE 表1[LEFT|RIGHT] JOIN 表2 ON 条件
SET 字段1=值1,字段2=值2,...;
```

### 6.3 DELETE语句

DELETE语句用于删除记录，语法如下:

```mysql
DELETE [IGNORE] FROM 表名
[WHERE 条件1,条件2,...]
[ORDER BY ...]
[LIMIT ...];
```

因为相关子查询效率非常低，所以我们可以利用表连接的方式来改造DELETE语句

```mysql
DELETE 表1,... FROM 表1 JOIN 表2 ON 条件
[WHERE 条件1,条件2,...]
[ORDER BY ...]
[LIMIT ...];
```

DELETE也支持外连接

```mysql
DELETE 表1,... FROM 表1[LEFT|RIGHT] JOIN 表2
ON 条件 ...;
```

DELETE语句是在事务机制下删除记录，删除记录之前，先把将要删除的记录保存到日志文件里，然后再删除记录。

TRUNCATE语句在事务机制之外删除记录，速度远超过DELETE语句

```mysql
TRUNCATE TABLE 表名;
```

## 七、MySQL函数

像编程语言利用函数封装业务功能一样，数据库也把一些复杂的功能封装到函数里，供使用者调用

### 7.1 数字函数

| 函数    | 用途             | 用例        |
| ------- | ---------------- | ----------- |
| ABS     | 绝对值           | ABS(-100)   |
| ROUND   | 四舍五入         | ROUND(4.62) |
| FLOOR   | 向下取整         | FLOOR(9.9)  |
| CEIL    | 向上取整         | CEIL(3.2)   |
| POWER   | 幂函数           | POWER(2,3)  |
| LOG     | 对数函数         | LOG(7,3)    |
| LN      | 对数函数         | LN(10)      |
| SQRT    | 开平方           | SQRT(9)     |
| PI      | 圆周率           | PI()        |
| SIN     | 正弦函数(弧度制) | SIN(1)      |
| COS     | 余弦函数         | COS(1)      |
| TAN     | 正切函数         | TAN(1)      |
| COT     | 余切函数         | COT(1)      |
| RADIANS | 角度转弧度       | RADIANS(30) |
| DEGREES | 弧度转角度       | DEGREES(1)  |

### 7.2 日期函数

| 函数        | 用途                                      | 用例                                    |
| ----------- | ----------------------------------------- | --------------------------------------- |
| NOW         | 获得系统日期时间，格式yyyy-MM-dd hh:mm:ss | NOW()                                   |
| CURDATE     | 获得当前系统日期，格式yyyy-MM-dd          | CURDATE()                               |
| CURTIME     | 获得当前系统时间，格式hh:mm:ss            | CURTIME()                               |
| DATE_FORMAT | 格式化日期，返回用户想要的日期格式        | DATE_FORMAT(日期，表达式)               |
| DATE_ADD    | 日期偏移计算                              | DATE_ADD(日期,INTERVAL 偏移量 时间单位) |
| DATEDIFF    | 计算两个日期之间的相差天数                | DATEDIFF(日期,日期)                     |

日期函数中表达式如下

![image-20210816235545157](http://static.codenote.xyz/img/20210816235545.png)

### 7.3 字符函数

| 函数      | 用途           | 用例                                                         |
| --------- | -------------- | ------------------------------------------------------------ |
| LOWER     | 转换小写字符   | LOWER(ename)                                                 |
| UPPER     | 转换大写字符   | UPPER(ename)                                                 |
| LENGTH    | 字符数量       | LENGTH(ename)                                                |
| CONCAT    | 连接字符串     | CONCAT(sal,"$")                                              |
| INSTR     | 字符出现的位置 | INSTR(ename,"A")                                             |
| INSERT    | 插入、替换字符 | INSERT("你好",1,0,"先生")第一个数字代表位置，第二个数字代表字符长度 |
| REPLACE   | 替换字符       | REPLACE("你好先生","先生","女士")                            |
| SUBSTR    | 截取字符串     | SUBSTR("你好世界",3,4)，第二个数字代表结束位置               |
| SUBSTRING | 截取字符串     | SUBSTRING("你好世界",3,2)，第二个数字代表字符偏移量          |
| LPAD      | 左侧填充字符   | LPAD("Hello",10,"*")                                         |
| RPAD      | 右侧填充字符   | RPAD("Hello",10,"*")                                         |
| TRIM      | 去除首尾空格   | TRIM("  你好先生  ")                                         |

### 7.4 条件函数

| 函数                        | 用途                                   | 用例               |
| --------------------------- | -------------------------------------- | ------------------ |
| IFNULL                      | 判断表达式是否为NULL，为NULL用值替换   | IFNULL(表达式,值)  |
| IF                          | 判断表达式是否成立，成立取值1，否则值2 | IF(表达式,值1,值2) |
| CASE-WHEN-THEN-ELSE-END语句 | 复杂的条件判断                         |                    |

## 八、事务

### 8.1 事务简介

事务是为了避免写入直接操作数据文件，数据的写入直接操作数据文件是非常危险的事情。

MySQL是通过日志文件来实现间接写入的。

MySQL一共有5中日志文件，其中只有undo日志和redo日志和事务有关。![image-20210817002927526](http://static.codenote.xyz/img/20210817002927.png)

RDBMS = SQL语句+事务（ACID)

事务是一个或者多个SQL语句组成的整体，要么全部执行成功，要么全都执行失败。

事务流程：开启事务	->	UPDATE语句、DELETE语句	->	提交事务

### 8.2 管理事务

默认情况下，MySQL执行每条SQL语句都会自动开启和提交事务

为了让多条SQL语句纳入到一个事务之下，可以手动管理事务

```mysql
START TRANSACTION;
# SQL 语句
[COMMIT|ROLLBACK];
```

### 8.3 事务的特性

原子性（atomic）：

一个事务中的所有操作要么全部完成，要么全部失败。事务执行后，不允许停留在中间某个状态。

一致性（consistent）：

不管在任何给定的时间、并发事务有多少，事务必须保证运行结果的一致性。

![image-20210817004205713](http://static.codenote.xyz/img/20210817004205.png)

不管怎么转账，总金额不变

隔离性（independent）：

要求事务不受其他并发事务的影响，如同在给定的时间内，该事务是数据库唯一运行的事物。

默认情况下A事务，只能看到日志中该事务的相关数据。

![image-20210817004257548](http://static.codenote.xyz/img/20210817004306.png)

持久性（durable）：

事务一旦提交，结果便是永久性的。即便发生宕机，仍然可以依靠事务日志完成数据的持久化

### 8.4 事务的并发性

事务的四个隔离级别

| 隔离级别         | 功能           |
| ---------------- | -------------- |
| read uncommitted | 读取未提交数据 |
| read committed   | 读取已提交数据 |
| repeatable read  | 重复读取       |
| serializable     | 序列化         |

在某些特殊场合，需要允许事务可以读取到临时数据，必须修改事务的隔离级别。

业务场景1：
在购票场景中，如果有一张票还未卖出，A事务先修改了票的状态但未提交事务，B事务看到票的状态还是未售出，也修改了票的状态并马上提交了事务，此时A就会回滚事务，导致购票失败。

解决方式：这时应该使用“read uncommitted”事务隔离级别，允许事务可以读取其他事务未提交的数据

```mysql
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
```

业务场景2：

在转账场景中，如果账户内有5000元，A事务开启，向账户内转账1000元，B事务支出100元。

如果A事务先开启，还没来得及做任何操作，此时B事务开启，将账户余额改为4900元，这时A事务开始执行转入1000元，如果设置隔离级别为“read uncommitted”，账户余额就修改为5900元，如果两个事务都正常提交，账户余额不会出现问题。但如果B事务出现问题，但是A事务已经提交，账户变为5900，B事务再回滚，账户仍然是5900，就会出现问题。

解决方式：

这时应该使用“read committed”隔离级别，代表只能读取其他事务提交的数据

```mysql
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

业务场景3：

在商城系统中，顾客在支付订单之前，商品涨价了。此时，先由A事务开启了下单购买，B事务开启了涨价。我们希望当前事务的执行不要受到其他事务的影响，甚至其他事务执行完了也不要影响到这个事务的执行。

解决方式：

这时应该使用“repeatable read”隔离级别，代表当前事务能读取的数据是事务开启以前的数据，得到的结果是一致的。事务开启之后，其他事务修改的数据此事务不受影响。这也是MySQL的默认事务隔离级别

```mysql
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

由于事务并发执行所带来的各种问题，前三种隔离级别只适用在某些业务场景中，但是序列化的隔离性，让事务逐一执行，就不会产生上述问题了。

```mysql
SET SESSION TRANSACTION ISOLATION
LEVEL
serializable;
```

虽然这样可以解决事务的执行顺序问题，但是这样会使事务的并发性急剧下降。

## 九、导入导出数据

### 9.1 数据导出和备份的区别

数据导出，导出的纯粹是业务数据

数据备份，备份的是数据文件、日志文件、索引文件等等；

![image-20210817010815456](http://static.codenote.xyz/img/20210817010815.png)

![image-20210817010841544](http://static.codenote.xyz/img/20210817010841.png)

### 9.2 导出为SQL文件

mysqldump用来把业务数据导出成SQL文件，其中也包括了表结构

```mysql
mysqldump -u root -p [no-data] 逻辑库 > 路径;
```

### 9.3 导入SQL文件

source命令用于导入SQL文件，包括创建数据表，写入记录等

必须在MySQL命令下才可以执行

```mysql
USE demo;
SOURCE xxx.sql;
```