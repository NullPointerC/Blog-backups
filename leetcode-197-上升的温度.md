---
title: leetcode-197-上升的温度
date: 2021-10-17 16:21:48
categories: [LeetCode]
tags:
  - SQL
  - LeetCode
---

[$link$](https://leetcode-cn.com/problems/rising-temperature/)

![image-20211017162235304](https://gitee.com/cao_ziqiang/img/raw/master/20211017162235.png)

<hr/>

![image-20211017162303295](https://gitee.com/cao_ziqiang/img/raw/master/20211017162303.png)

<hr/>

- $Weather$表自连接，连接条件为日期间隔1天且温度低于第二天的温度；
- 使用$MySQL$的$DATEDIFF$函数计算时间间隔；

```mysql
# Write your MySQL query statement below
select 
    w2.id as id
from Weather w1 join Weather w2
on DATEDIFF(w2.recordDate, w1.recordDate) = 1
and w1.Temperature < w2.Temperature
```

