---
title: leetcode-176-第二高的薪水
date: 2021-08-25 11:02:07
categories: LeetCode
tags: [MySQL,SQL,LeetCode]
---

[link](https://leetcode-cn.com/problems/second-highest-salary/)

<hr/>

题目描述:

<pre>
编写一个 SQL 查询，获取 Employee 表中第二高的薪水（Salary） 。
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
例如上述 Employee 表，SQL查询应该返回 200 作为第二高的薪水。如果不存在第二高的薪水，那么查询应返回 null。
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
</pre>

思路:

<pre>
一开始我的思路是内连接,先把原表中的最高薪水查出来,然后作为关联表查询
</pre>

```mysql
# Write your MySQL query statement below
SELECT
e.Salary as SecondHighestSalary
FROM
Employee e INNER JOIN
(SELECT MAX(Salary) highest FROM Employee) m
ON
e.Salary < m.highest
ORDER BY Salary DESC
LIMIT 1;
```

<pre>
但是这样有个问题就是如果不存在第二高的薪水,就没有返回值,但是题目要求是,如果不存在第二高的薪水,应该要返回null,上面的查询结果在ON字句筛选时,就过滤掉了不符合条件的,所以最后连null值也没有返回。
</pre>

<pre>
后来想到题目也没有想的那么复杂，可以先将薪水去重并按降序排序，
然后从第一条记录开始查询一条记录即可。
</pre>

```mysql
SELECT
	IFNULL(
      (SELECT 
       DISTINCT Salary
       FROM Employee
       ORDER BY Salary DESC
       LIMIT 1 OFFSET 1
      )
      ,NULL
    ) 
AS SecondHighestSalary;
```

<pre>
也可以继续使用聚合函数，先将max查询出来，再去查询第二个max，但是这样的查询有局限性，如果扩展至第n高，则需要嵌套多个子查询，SQL执行效率降低
</pre>

```mysql
select max(Salary) SecondHighestSalary 
from Employee
where Salary < (select max(Salary) from Employee)
```

![image-20210825122114993](https://gitee.com/cao_ziqiang/img/raw/master/20210825122115.png)

