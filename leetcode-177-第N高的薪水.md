---
title: leetcode-177-第N高的薪水
date: 2021-08-29 10:14:10
categories: LeetCode
tags: [MySQL,SQL,LeetCode]
---

[link](https://leetcode-cn.com/problems/nth-highest-salary/)

<hr/>

题目描述:

<pre>
编写一个 SQL 查询，获取 Employee 表中第 n 高的薪水（Salary）。
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
例如上述 Employee 表，n = 2 时，应返回第二高的薪水 200。如果不存在第 n 高的薪水，那么查询应返回 null。
+------------------------+
| getNthHighestSalary(2) |
+------------------------+
| 200                    |
+------------------------+
</pre>

思路:

<pre>
和176题一样的思路,先对数据去重,并且注意判断临界情况,如果不存在,需要返回null;
再使用limit和offset设置分页
</pre>

```mysql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
SET N = N - 1;
  RETURN (
      # Write your MySQL query statement below.
    SELECT
	    IFNULL(
            (   
                SELECT 
                DISTINCT Salary
                FROM Employee
                ORDER BY Salary DESC
                LIMIT 1 OFFSET N
            ),
            NULL
        ) AS getNthHighestSalary
  );
END
```

恕我直言,我用了2年MySQL没听过自定义函数这个玩意,太神奇了,刚在leetcode的IDE上看到的时候还愣住了...

![image-20210829101937916](https://gitee.com/cao_ziqiang/img/raw/master/20210829101938.png)