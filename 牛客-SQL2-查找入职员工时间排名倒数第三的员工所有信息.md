---
title: 牛客-SQL2-查找入职员工时间排名倒数第三的员工所有信息
categories:
  - nowcoder
tags:
  - SQL
  - nowcoder
hidden: true
abbrlink: 84ca03a5
date: 2021-09-26 20:33:34
---

[$link$](https://www.nowcoder.com/practice/ec1ca44c62c14ceb990c3c40def1ec6c?tpId=82&&tqId=29754&rp=1&ru=/activity/oj&qru=/ta/sql/question-ranking)

<hr/>

![image-20210926203440306](https://gitee.com/cao_ziqiang/img/raw/master/20210926203440.png)

<hr/>

先使用$order\quad by$按入职时间降序排序,再跳过前面2条记录即可。

```mysql
select * from employees order by `hire_date` desc limit 1 offset 2;
```

