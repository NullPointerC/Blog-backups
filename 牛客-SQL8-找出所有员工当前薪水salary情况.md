---
title: 牛客-SQL8-找出所有员工当前薪水salary情况
categories:
  - nowcoder
tags:
  - SQL
  - nowcoder
abbrlink: 995cff6b
date: 2021-10-07 14:09:07
---



[$link$](https://www.nowcoder.com/practice/ae51e6d057c94f6d891735a48d1c2397?tpId=82&&tqId=29760&rp=1&ru=/activity/oj&qru=/ta/sql/question-ranking)

## 描述

有一个薪水表，salaries简况如下:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211007140944.png)

请你找出所有员工具体的薪水salary情况，对于相同的薪水只显示一次,并按照逆序显示，以上例子输出如下:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211007140953.png)

<hr/>

把所有的$salary$查询出来再进行去重即可,并且根据$salary$降序排列

```mysql
select distinct salary
from salaries
order by salary desc;
```

