---
title: 牛客-SQL3-查找当前薪水详情以及部门编号dept_no
categories:
  - nowcoder
tags:
  - SQL
  - nowcoder
hidden: true
abbrlink: 8cc67f3e
date: 2021-09-26 20:42:17
---

[$link$](https://www.nowcoder.com/practice/c63c5b54d86e4c6d880e4834bfd70c3b?tpId=82&&tqId=29755&rp=1&ru=/activity/oj&qru=/ta/sql/question-ranking)

<hr/>

![image-20210926204307409](http://static.codenote.xyz/img/20210926204307.png)

![image-20210926204324003](http://static.codenote.xyz/img/20210926204324.png)

<hr/>

根据$dept\text{_}manager$表中的$emp\text{_}no$与$salaries$表中的$emp\text{_}no$字段做内连接即可。

```mysql
select 
    sal.emp_no, sal.salary, sal.from_date,sal.to_date,dm.dept_no
from 
dept_manager dm, salaries sal 
where dm.emp_no = sal.emp_no
ORDER by sal.emp_no
```

