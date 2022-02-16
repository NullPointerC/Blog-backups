---
title: leetcode-181-超过经理收入的员工
categories:
  - LeetCode
tags:
  - SQL
  - LeetCode
abbrlink: a97c412e
date: 2021-10-10 18:43:59
---

[$link$](https://leetcode-cn.com/problems/employees-earning-more-than-their-managers/)

![image-20211010184428850](https://gitee.com/cao_ziqiang/img/raw/master/20211010184428.png)

<hr/>

- $Employee$表与自身表连接，且连接条件为$e1.ManagerId = e1.Id$
- $where$过滤条件为$e1.Salary \gt e2.Salary$

```mysql
# Write your MySQL query statement below

SELECT
    e1.Name as Employee
FROM Employee e1 JOIN Employee e2
ON e1.ManagerId = e2.Id
WHERE e1.Salary > e2.Salary
```

