---
title: leetcode-89-格雷编码
date: 2022-01-08 11:28:19
categories: LeetCode
tags: [LeetCode,algorithm]
---

[89. 格雷编码](https://leetcode-cn.com/problems/gray-code/)

![image-20220108112856942](https://gitee.com/cao_ziqiang/img/raw/master/20220108112857.png)

学习一下怎么求格雷编码。

一个比较简单的办法就是直接暴力枚举即可。

```java
class Solution {
    public List<Integer> grayCode(int n) {
        if(n <= 0) {
            return null;
        }
        boolean[] st = new boolean[1 << n];
        List<Integer> ans = new ArrayList<>(1 << n);
        ans.add(0);
        st[0] = true;
        while(ans.size() != (1 << n)) {
            int last = ans.get(ans.size() - 1);
            for(int j = 0; j < n; j++) {
                if(!st[(1 << j) ^ last]) {
                    ans.add((1 << j) ^ last);
                    st[(1 << j) ^ last] = true;
                    break;
                }
            }
        }
        return ans;
    }
}
```

