---
title: leetcode-175-组合两个表
categories:
  - LeetCode
tags:
  - SQL
  - LeetCode
abbrlink: 1b367fea
date: 2021-08-23 15:40:19
---

[link](https://leetcode-cn.com/problems/combine-two-tables/)

<hr/>

题目描述

<pre>
表1: Person
+-------------+---------+
| 列名         | 类型     |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
PersonId 是上表主键
表2: Address
+-------------+---------+
| 列名         | 类型    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
AddressId 是上表主键
编写一个 SQL 查询，满足条件：无论 person 是否有地址信息，都需要基于上述两表提供 person 的以下信息：
 FirstName, LastName, City, State
</pre>

思路:

<pre>
这里说不管person中是否有地址信息都要提供person的信息,很自然让我们想到
对person表进行左连接address表查询,查询时,要注意on和where的区别
数据库在通过连接两张或多张表来返回记录时，都会生成一张中间的临时表，然后再
将这张临时表返回给用户。 在使用left jion时，on和where条件的区别如下：
1、on条件是在生成临时表时使用的条件，它不管on中的条件是否为真，都会返回左
边表中的记录。
2、where条件是在临时表生成好后，再对临时表进行过滤的条件。这时已经没有
left join的含义（必须返回左边表的记录）了，条件不为真的就全部过滤掉。
</pre>

代码如下:

```mysql
# Write your MySQL query statement below
SELECT
p.FirstName,p.LastName,a.City,a.State
FROM
Person p LEFT JOIN
Address a
on p.PersonId = a.PersonId
```

![image-20210823154429168](https://gitee.com/cao_ziqiang/img/raw/master/20210823154429.png)