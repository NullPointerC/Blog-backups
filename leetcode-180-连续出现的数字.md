---
title: leetcode-180-连续出现的数字
categories: [LeetCode]
tags:
  - SQL
  - LeetCode
date: 2021-10-08 10:16:54
---

[$link$](https://leetcode-cn.com/problems/consecutive-numbers/)

<hr/>

![image-20211008101730264](https://gitee.com/cao_ziqiang/img/raw/master/20211008101730.png)

![image-20211008101747039](https://gitee.com/cao_ziqiang/img/raw/master/20211008101747.png)

<hr/>

可将$Logs$表自连接$3$次,若为连续出现的数字,则$id$必然为自增.且$num$值相同

故逻辑如下:

```mysql
# Write your MySQL query statement below
select distinct(l.Num) ConsecutiveNums 
from Logs l 
inner join Logs l2 on l.Num = l2.Num and  l.id = l2.id+1
inner join Logs l3 on l.Num = l3.Num and  l.id = l3.id+2
```

