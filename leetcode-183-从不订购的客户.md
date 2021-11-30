---
title: leetcode-183-从不订购的客户
categories: [LeetCode]
tags:
  - SQL
  - LeetCode
date: 2021-10-10 19:28:47
---

## 题目

[$link$](https://leetcode-cn.com/problems/customers-who-never-order/)

<hr/>

![image-20211010192920563](https://gitee.com/cao_ziqiang/img/raw/master/20211010192920.png)

<hr/>

- 首先从$Orders$表中查询出有过订购记录的用户$Id$;
- 再从$Customers$表中查询出其$Id$不在上述$Id$中的用户名称；

```mysql
# Write your MySQL query statement below
select
    c1.Name as `Customers`
from Customers c1
where c1.Id not in (select CustomerId from Orders);
```

- 从评论区看到如果用$in$查询的话，时间复杂度会达到$log(mn)$级别，且使用$in$查询不会走索引过；
- 而使用连接查询，时间复杂度会是$m\times lg(n) + n \times lg(m)$，也可以走索引过；

```mysql
select Name as Customers 
	from Customers left join Orders 
on Customers.Id = Orders.CustomerID 
where Orders.CustomerID is null
```

<hr/>

从上次惨痛的面试经历告诉我，很多事情不是了解是这样就可以，更应该去深究其底层原理，知道了是什么之外，更应该知道为什么！

## 不推荐使用`in`的原因

### 字段的唯一性不高

当字段的唯一性不高时，这个字段即使在数据量很大，但是值只有固定那些，这种情况下影响结果集巨大，就会进行全表扫描，这时全表扫描的效率还要高于使用索引。

加入90万的数据，某字段$source$只有0、1两个值，利用索引还要先读取索引文件，然后再去二分查找，找到对应数据的磁盘指针，再根据磁盘指针去读磁盘上的数据，影响结果集巨大，这种情况下反而不如直接进行全表扫描。

同时，数据区分度不高的字段上不宜建立索引。区分度：`count(discint(列名))/count(列名)`。

区分度表示字段不重复的比例，比例越大，扫描的记录数越小，唯一键的区分度是1，而一些状态、性别字段在大数据面前区分度约等于0。对于区分度不大的字段，建立索引并没有很大的意义，不能有效过滤数据，性能与全表扫描。其次，建立索引也是有消耗的，要根据业务场景来选择合适的索引。



参考链接：

- https://www.zhihu.com/question/26010353