---
title: 牛客-SQL7-查找薪水记录超过15次的员工号emp_no以及其对应的记录次数t
categories:
  - nowcoder
tags:
  - SQL
  - nowcoder
hidden: true
abbrlink: 378d5a6c
date: 2021-10-07 14:03:46
---

[$link$](https://www.nowcoder.com/practice/6d4a4cff1d58495182f536c548fee1ae?tpId=82&tags=&title=&difficulty=0&judgeStatus=0&rp=1)

<hr/>

## 描述

有一个薪水表，salaries简况如下:

![img](http://static.codenote.xyz/img/20211007140527.png)

请你查找薪水记录超过15次的员工号emp_no以及其对应的记录次数t，以上例子输出如下:

![img](http://static.codenote.xyz/img/20211007140534.png)

<hr/>

先根据$emp\text_no$即可,再使用$count$聚合函数统计

```mysql
select 
    emp_no, count(emp_no) as t
from salaries
GROUP by emp_no HAVING count(emp_no) > 15;
```

