---
title: 牛客-SQL4-查找所有已经分配部门的员工的last_name和first_name以及dept_no
categories:
  - nowcoder
tags:
  - SQL
  - nowcoder
hidden: true
abbrlink: f9f9351c
date: 2021-09-27 21:57:45
---

[link](https://www.nowcoder.com/practice/6d35b1cd593545ab985a68cd86f28671?tpId=82&&tqId=29756&rp=1&ru=/activity/oj&qru=/ta/sql/question-ranking)

<hr/>

有一个员工表，employees简况如下:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210927215828.png)

有一个部门表，dept_emp简况如下:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210927215839.png)

请你查找所有已经分配部门的员工的last_name和first_name以及dept_no，未分配的部门的员工不显示，以上例子如下:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210927215845.png)

<hr/>

对$dept\text{_}emp$表和$employees$做内连接即可，条件为$dept\text{_}emp.emp\text{_}no=employees.emp\text{_}no$

```mysql
select 
    emp.last_name,emp.first_name, dm.dept_no
from 
dept_emp dm
join
employees emp 
on dm.emp_no = emp.emp_no;
-- 查找所有已经分配部门的员工的last_name和first_name以及dept_no
```

