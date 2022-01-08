---
title: leetcode-1705-吃苹果的最大数目
date: 2021-12-24 15:32:54
categories: LeetCode
tags: [LeetCode,algorithm]
---

[吃苹果的最大数目](https://leetcode-cn.com/problems/maximum-number-of-eaten-apples/)

<hr/>

![image-20211224153355828](https://gitee.com/cao_ziqiang/img/raw/master/20211224153355.png)

tags: (pq, 贪心)

<hr/>

比较有创意的$pq$运算以及贪心。

我们考虑优先吃掉快过期的苹果是最优解，维护这个苹果过期的过程，可以用$pq$来维护。

苹果的数量很大，但是产生苹果的天数最多为$2 \times 10 ^ 4$，所以可以创建一个新的$apple$数组，第一个值存储当天产生的苹果数量$apples[i]$，第二个值存储当前产生的苹果的过期时间$i+days[i]$，其中$i$为当前天数。

- 当$i \lt n$时，还能够继续产生苹果，所以要继续模拟；
- 能产生苹果时，将当天的苹果以二元组$(apples[i],i+days[i])$存进$pq$中；
- 然后从堆中取出最后食用日期中最早可食用的苹果，如果堆顶元素已经过期，即$pq.peek()[1]<i$，则抛弃堆顶的苹果，吃掉堆顶一个苹果后，如果仍有剩余，并且最后时间大于当前的时间（尚未过期），将堆顶重新入堆；
- 循环上述过程，直到所有的苹果都出堆。

```java
class Solution {
    public int eatenApples(int[] apples, int[] days) {
        int ans = 0;
        int n = apples.length;
        Queue<int[]> pq = new PriorityQueue<>((a, b) -> Integer.compare(a[1], b[1]));
        int i = 0;
        for(; i < n; i++) {
            // 建立新的apples数组,下标0为改组苹果数量,下标1位改组苹果过期时间
            // 将苹果放入pq中,按过期时间由早到晚排序
            if(apples[i] > 0) {
                int[] newApples = {apples[i], i + days[i]};
                pq.offer(newApples);
            }
            while(!pq.isEmpty() && pq.peek()[1] <= i) {
                // 清除到期限的苹果
                pq.poll();
            }
            if(!pq.isEmpty()) {
                // 优先吃容易坏的的苹果
                ans++;
                if(--pq.peek()[0] == 0) {
                    // 苹果吃完了
                    pq.poll();
                }
            }
        }
        // 过了n天,不长新苹果了,开始吃剩下的苹果
        do {
            while(!pq.isEmpty() && pq.peek()[1] <= i) {
                pq.poll();
            }
            if(!pq.isEmpty()) {
                int amount = Math.min(pq.peek()[0], pq.peek()[1] - i);
                ans += amount;
                i += amount;
                pq.poll();
            }
        } while(!pq.isEmpty());
        return ans;
    }
}
```

