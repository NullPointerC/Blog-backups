---
title: 牛客-SQL11-获取所有员工当前的manager
categories:
  - nowcoder
tags:
  - SQL
  - nowcoder
hidden: true
abbrlink: 182cfafe
date: 2021-10-07 14:21:29
---

[$link$](https://www.nowcoder.com/practice/e50d92b8673a440ebdf3a517b5b37d62?tpId=82&tags=&title=&difficulty=0&judgeStatus=0&rp=1)

<hr/>

## 描述

有一个员工表dept_emp简况如下:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211007142159.png)

第一行表示为员工编号为10001的部门是d001部门。

有一个部门经理表dept_manager简况如下:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211007142204.png)

第一行表示为d001部门的经理是编号为10002的员工。

获取所有的员工和员工对应的经理，如果员工本身是经理的话则不显示，以上例子如下:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211007142208.png)

<hr/>

首先我们先明确了$dept\text_manager$表中只有部门编号以及部门经理的员工编号,所以可以根据$dept\text_emp$表中的$dept\text_no$字段与$dept\text_no$字段做内连接,找出所属于的部门并且因为员工是经理的话本身不需要显示,所以再加上一个判断条件为$dept\text_emp$表中的$dept\text_no$与$dept\text_manager$中的$dept\text_no$字段不等即可

```mysql
select 
    dept_emp.emp_no as emp_no, dept_manager.emp_no as manager
from dept_emp INNER join dept_manager
on dept_emp.dept_no = dept_manager.dept_no and dept_emp.emp_no != dept_manager.emp_no;
```

