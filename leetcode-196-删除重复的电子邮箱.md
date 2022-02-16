---
title: leetcode-196-删除重复的电子邮箱
categories:
  - LeetCode
tags:
  - SQL
  - LeetCode
abbrlink: cb699d5c
date: 2021-10-11 10:16:08
---

[$link$](https://leetcode-cn.com/problems/delete-duplicate-emails/)

<hr/>

![image-20211011101722661](https://gitee.com/cao_ziqiang/img/raw/master/20211011101722.png)

<hr/>

- 需要用到$delete$删除重复行;
- 使用$Person$与自身自连接,删除的条件为$p1.Eamil=p2.Email \& p1.Id \gt p2.Id$

```mysql
# Write your MySQL query statement below
delete
    p1
from Person p1, Person p2
where p1.Email = p2.Email and p1.Id > p2.Id
```

