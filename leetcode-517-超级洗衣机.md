---
title: leetcode-517-超级洗衣机
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-29 11:24:58
---

[$link$](https://leetcode-cn.com/problems/super-washing-machines/)

<hr/>

![image-20210929112550479](https://gitee.com/cao_ziqiang/img/raw/master/20210929112550.png)


题目意思为要使数组每个元素的值都相同，需要进行的次数。同时数组允许在$m$个值中，它们互相送$1$件衣服给相邻元素。

很容易判断出不符合的情况：$sum(machines) \textit% len(machine) \ne 0$，此时就不符合平均分配，可以直接返回$-1$。

对于每台洗衣机而言，它自身要达到相等的条件就是移动$machines[i] - avg$件衣服，对于整个过程来说，最大值也就是$max(machines[i]) - avg$。

而对于整体来说，将这台洗衣机和它后面的洗衣机做一个分组，前面共计$i$台洗衣机都要达到相等的条件为移动$preSum(i) - i \times avg$件衣服。

所以再判断同时满足自身和整体两个条件下的最大值，这就是转移的最少次数。

```java
class Solution {
    public int findMinMoves(int[] machines) {
        int ans = 0;
        int n = machines.length;
        if(n == 0) {
            return ans;
        }
        // 前缀和,preSum[i] = machines[0...i];
        int[] preSum = new int[n];
        preSum[0] = machines[0];
        for (int i = 1; i < n; i++) {
            preSum[i] = preSum[i - 1] + machines[i];
        }
        
        if (preSum[n - 1] % n != 0) return -1;
        int avg = preSum[n - 1] / n;
        for (int i = 0; i < n; i++) {
            int step = Math.max(machines[i] - avg, Math.abs(preSum[i] - (i + 1) * avg));
            ans = Math.max(step, ans);
        }
        return ans;
    }
}
```

上面的思路不好理解的话也可以尝试理解平衡条件下，左边需要移入/移出的衣服件数 = 右边需要移入/移出的衣服件数。

```java
class Solution {
    public int findMinMoves(int[] machines) {
        int n = machines.length;
        if(n == 0) {
            return 0;
        }
        int[] preSum = new int[n];
        preSum[0] = machines[0];
        for(int i = 1; i < n; i++){
            preSum[i] = preSum[i - 1] + machines[i];
        }
        if(preSum[n-1] % n != 0) return -1;
        int avg = preSum[n - 1] / n;
        int ans = 0;
        for(int i = 0; i < n; i++){
            int leftNeed = avg * i;
            int rightNeed = avg * (n - i - 1);
            // 左边的所有衣服之和
            int leftSum = i > 0 ? preSum[i - 1] : 0;
            // 右边的所有衣服之和
            int rightSum = preSum[n - 1] - preSum[i];
            leftNeed -= leftSum;
            rightNeed -= rightSum;
            if(leftNeed > 0 &&  rightNeed > 0) 
                ans = Math.max(ans, leftNeed + rightNeed);
            else ans = Math.max(ans, Math.max(Math.abs(leftNeed), Math.abs(rightNeed)));
        }
        return ans;
    }
}
```

