---
title: 牛客-SQL10-获取所有非manager的员工emp_no
categories:
  - nowcoder
tags:
  - SQL
  - nowcoder
hidden: true
abbrlink: 3b2cc27c
date: 2021-10-07 14:14:54
---

[$link$](https://www.nowcoder.com/practice/32c53d06443346f4a2f2ca733c19660c?tpId=82&tags=&title=&difficulty=0&judgeStatus=0&rp=1)

<hr/>

## 描述

有一个员工表employees简况如下:

![img](http://static.codenote.xyz/img/20211007141540.png)

有一个部门领导表dept_manager简况如下:

![img](http://static.codenote.xyz/img/20211007141603.png)

请你找出所有非部门领导的员工emp_no，以上例子输出:

![img](http://static.codenote.xyz/img/20211007141606.png)

<hr/>

<hr/>

可以先将所有是$manager$的员工查询出来作为一张临时表,再使用$not$和$in$关键字查询出所有非$manager$的$emp\text_no$

```mysql
SELECT
    emp.emp_no
from employees emp
where emp.emp_no not in (select emp_no from dept_manager);
```

