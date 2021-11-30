---
title: leetcode-178-分数排名
categories: [LeetCode]
tags:
  - SQL
  - LeetCode
date: 2021-09-22 20:39:36
---

[$link$](https://leetcode-cn.com/problems/rank-scores/)

<hr/>

![image-20210922204113069](https://gitee.com/cao_ziqiang/img/raw/master/20210922204113.png)

![image-20210922204125851](https://gitee.com/cao_ziqiang/img/raw/master/20210922204125.png)

<hr/>

如果不考虑性能的话，可以直接上子查询。

```mysql
# Write your MySQL query statement below
SELECT Score, (SELECT count(DISTINCT score) FROM Scores WHERE score >= s.score) AS 'Rank' FROM Scores s ORDER BY Score DESC ;
```

这里结合了子查询和自关联。先进行去重，然后再查询数量就是排名了。

<hr/>

后来看到评论区有人提到$mysql8$提供了内置函数：$dense\text{_}rank$。

![image-20210922204531141](https://gitee.com/cao_ziqiang/img/raw/master/20210922204531.png)

语法如上，所以$SQL$语句可以改写为：

```mysql
select Score, dense_rank() over(Order By Score desc) 'Rank' FROM Scores;
```

