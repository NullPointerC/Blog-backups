---
title: leetcode-182-查找重复的电子邮箱
categories:
  - LeetCode
tags:
  - SQL
  - LeetCode
abbrlink: e3d7e3a9
date: 2021-10-10 18:49:08
---

[$link$](https://leetcode-cn.com/problems/duplicate-emails/)

<hr/>

![image-20211010184939178](https://gitee.com/cao_ziqiang/img/raw/master/20211010184939.png)

<hr/>

- 选择$Person$表与自身表连接，连接条件为两者$Email$相同；
- 过滤条件为二者$Id$不同，并设置$distinct$去重；

```mysql
# Write your MySQL query statement below
select 
    distinct p1.Email
from Person p1 join Person p2 on p1.Email = p2.Email
where p1.Id <> p2.Id;
```

