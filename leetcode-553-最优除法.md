---
title: leetcode-553-最优除法
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: ea8693d2
date: 2022-02-27 11:50:33
---

[553. 最优除法](https://leetcode-cn.com/problems/optimal-division/)

![image-20220227115109481](https://gitee.com/cao_ziqiang/img/raw/master/20220227115109.png)

说实话没有什么思路刚看到这题，以为是回溯所有结果，后面看答案，发现思路也太简洁了。

假设最后的结果形式是
$$
a \div b
$$
要使这个结果最大，就应该要让$b$尽可能的小，再观察数据范围，

发现数据范围都在
$$
[2,1000]
$$
这个区间内，所以应该让尽可能多的数参与到分子中，转换为连乘，
$$
\frac{nums[0]}{nums[1] / nums[2] / num[3] / ... / nums[n - 2] / num[n - 1]}
$$
再转换为连乘
$$
\frac{nums[0]}{nums[1] \times \frac{1}{nums[2]} \times \frac{1}{nums[3]} \times ... \times \frac{1} {nums[n-2]} \times \frac{1}{nums[n-1]}}
$$
也就是
$$
\frac{nums[0] \times nums[2] \times nums[3] \times ... nums[n - 2] \times nums[n-1]}{nums[1]}
$$

```java
class Solution {
    public String optimalDivision(int[] nums) {
        StringBuilder sb = new StringBuilder();
        int n = nums.length;
        sb.append(nums[0]);
        if(n == 1) return sb.toString(); 
        sb.append("/");
        if(n == 2) {
            sb.append(nums[1]);
            return sb.toString();
        }
        sb.append("(");
        for(int i = 1; i < nums.length; i++) {
            sb.append(nums[i]);
            if(i != nums.length - 1) {
                sb.append("/");
            }
        }
        sb.append(")");
        return sb.toString();
    }
}
```

