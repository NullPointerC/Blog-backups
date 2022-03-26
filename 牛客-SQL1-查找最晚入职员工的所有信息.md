---
title: 牛客-SQL1-查找最晚入职员工的所有信息
categories:
  - nowcoder
tags:
  - SQL
  - nowcoder
hidden: true
abbrlink: 472065a7
date: 2021-09-25 10:52:01
---

[link](https://www.nowcoder.com/practice/218ae58dfdcd4af195fff264e062138f?tpId=82&&tqId=29753&rp=1&ru=/activity/oj&qru=/ta/sql/question-ranking)

<hr/>

![image-20210925105313036](http://static.codenote.xyz/img/20210925105313.png)

<hr/>

<hr/>

最简单的办法就是使用$order\quad by$按$hire\text{_}date$降序排列并输出一条即可。

```mysql
SELECT * FROM employees order by hire_date desc limit 0,1;
```

也可以先使用$max$聚合函数求出最晚的入职时间，再求出入职时间等于最晚的即可。

```mysql
SELECT * FROM employees WHERE hire_date == (SELECT MAX(hire_date) FROM employees);
```

