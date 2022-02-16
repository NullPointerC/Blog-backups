---
title: leetcode-184-部门工资最高的员工
categories:
  - LeetCode
tags:
  - SQL
  - LeetCode
abbrlink: 11615f09
date: 2021-10-10 19:16:28
---

[$link$](https://leetcode-cn.com/problems/department-highest-salary/)

<hr/>

![image-20211010191714833](https://gitee.com/cao_ziqiang/img/raw/master/20211010191714.png)

<hr/>

- 先根据部门编号分组，并查询出每个部门的最高工资以及部门编号；
- 再查询$Employee$表与$Department$表连接的结果，连接条件为二者$Id$相同，目的是找出部门名称；
- 在根据前者找出的所有员工的部门编号以及工资去第一步中找出的部门编号以及部门最高工资相比较；

最终的结果就是我们所需要求得的部门工资最高的员工

```mysql
# Write your MySQL query statement below
select 
    d.Name as `Department`, e.Name as `Employee`, e.Salary as `Salary`
from
Employee e join Department d on e.DepartmentId = d.Id
where (e.DepartmentId, e.Salary) in 
(select 
    e.DepartmentId, max(e.Salary)
from Employee e group by e.DepartmentId);
```

