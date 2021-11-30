---
title: leetcode-119-杨辉三角Ⅱ
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-07-16 21:19:44
---

[link](https://leetcode-cn.com/problems/pascals-triangle-ii/)

<hr/>

## 题目描述:

```html
给定一个非负索引 k，其中 k ≤ 33，返回杨辉三角的第 k 行。
在杨辉三角中，每个数是它左上方和右上方的数的和。
```

## 示例:

<pre>
输入: 3
输出: [1,3,3,1]
</pre>

## 思路:

<pre>
比较常规的dp,注意这里有坑即可，这里要的是索引,所以从0开始
</pre>

## 代码：

```java
class Solution {
    public List<Integer> getRow(int rowIndex) {
        if (rowIndex < 0) {
            return null;
        }
        List<Integer> prevRow = new ArrayList<>();
        List<Integer> nowRow = null;
        for (int i = 1; i <= rowIndex + 1; i++) {
            nowRow = new ArrayList<>();
            for (int j = 1; j <= i; j++) {
                if (j == 1 || j == i) {
                    nowRow.add(1);
                } else {
                    nowRow.add(prevRow.get(j - 2) + prevRow.get(j - 1));
                }
            }
            prevRow = nowRow;
        }
        return nowRow;
    }
}
```

![image-20210716213947849](https://gitee.com/cao_ziqiang/img/raw/master/20210716213948.png)

