---
title: 牛客-SQL5-查找所有员工的last_name和first_name以及对应部门编号dept_no
categories:
  - nowcoder
tags:
  - SQL
  - nowcoder
abbrlink: bdb860dd
date: 2021-10-07 13:58:51
---

[$link$](https://www.nowcoder.com/practice/dbfafafb2ee2482aa390645abd4463bf?tpId=82&&tqId=29757&rp=1&ru=/activity/oj&qru=/ta/sql/question-ranking)

<hr/>

![image-20211007135932454](https://gitee.com/cao_ziqiang/img/raw/master/20211007135932.png)

<hr/>

由结果可知, 对于$dept\text{_}no$需要保留,所以可以对$employee$表与$dept\text{_}emp$表做左连接

```mysql
select 
    emp.last_name as last_name, emp.first_name as first_name, de.dept_no as dept_no
from employees emp 
left join dept_emp de
on emp.emp_no = de.emp_no;
```

