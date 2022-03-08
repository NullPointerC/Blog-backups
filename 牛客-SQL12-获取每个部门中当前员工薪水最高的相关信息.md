---
title: 牛客-SQL12-获取每个部门中当前员工薪水最高的相关信息
categories:
  - nowcoder
tags:
  - SQL
  - nowcoder
hidden: true
abbrlink: ff3d287a
date: 2021-10-08 21:17:08
---

[$link$](https://gitee.com/cao_ziqiang/img/raw/master/20211008211743.png)

## 描述

有一个员工表dept_emp简况如下:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211008211743.png)

有一个薪水表salaries简况如下:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211008211805.png)

获取每个部门中当前员工薪水最高的相关信息，给出dept_no, emp_no以及其对应的salary，按照部门编号dept_no升序排列，以上例子输出如下:

（注意: Mysql与Sqlite select 非聚合列的结果可能不一样)

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211008211811.png)

<hr/>

- 先获取每个人的工资, 使用$dept\text_emp$表和$salaries$内连接即可,连接条件为$dept\text_emp.emp\text_no=salaries.emp\text_no$,并且要注意题目要求的是当前,所以$to\text_date='9999-01-01'$
- 查询出上述信息后,再查询(部门,工资)$in$(部门,最高工资)

```mysql
select d.dept_no,d.emp_no,s.salary
from dept_emp as d
join salaries as s
on d.emp_no = s.emp_no
and d.to_date='9999-01-01'
and s.to_date='9999-01-01'
where (d.dept_no,s.salary) in (
    select d.dept_no,max(s.salary)
        from dept_emp as d
        join salaries as s
        on d.emp_no = s.emp_no
        and d.to_date='9999-01-01'
        and s.to_date='9999-01-01'
        group by d.dept_no
    )
order by d.dept_no
```

<hr/>

- 也可以考虑先将每个部门的最高工资以及对应的部门先找出来,形成为表$t2$;
- 再将每个人对应的工资及对应的部门找出来形成表$t1$;
- 再将$t1$与$t2$做表连接,条件为部门相同且工资相同;

```mysql
select t2.dept_no,t1.emp_no,t2.maxSalary
from
-- t1表——通过连接两个表找到每个人对应的工资和部门
(select s.emp_no,d.dept_no,s.salary from
dept_emp as d join salaries as s on d.emp_no=s.emp_no
and d.to_date='9999-01-01'
and s.to_date='9999-01-01') 
as t1 join
-- t2表——通过连接两个表找到每个部门对应的最高工资
(select d.dept_no,max(s.salary)as maxSalary from 
dept_emp d join salaries as s on d.emp_no =s.emp_no 
and d.to_date='9999-01-01'
and s.to_date='9999-01-01'
group by d.dept_no) 
as t2
-- 通过部门编号来进行连接两个生成的表
on t1.dept_no=t2.dept_no and t1.salary=t2.maxSalary 
-- 按照部门编号升序排列
order by t2.dept_no
```

