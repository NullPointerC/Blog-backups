---
title: leetcode-1748-唯一元素的和
date: 2022-02-06 00:23:41
categories: LeetCode
tags: [LeetCode,algorithm]
---

[1748. 唯一元素的和](https://leetcode-cn.com/problems/sum-of-unique-elements/)

![image-20220206002432562](https://gitee.com/cao_ziqiang/img/raw/master/20220206002432.png)

最近的办法自然就是哈希计数法了。

```java
class Solution {
    public int sumOfUnique(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        for(var num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        int res = 0;
        for(var k : map.entrySet()) {
            if(map.get(k.getKey()) == 1) res += k.getKey();
        }
        return res;
    }
}
```

