---
title: leetcode-846-一手顺子
date: 2021-12-30 10:59:37
categories: LeetCode
tags: [LeetCode,algorithm]
---

[846. 一手顺子](https://leetcode-cn.com/problems/hand-of-straights/)

![image-20211230105956808](https://gitee.com/cao_ziqiang/img/raw/master/20211230105956.png)

<hr/>

tags: (排序+map)

这里可以使用$Java$中的$TreeMap$来进行计数。可以看出每一组有$groupSize$张牌，于是就有$n / groupSize$组，对于每一组，都需要有$groupSize$张连续的牌。

考虑先对这些牌进行计数，统计每张牌出现的频率，再尝试每次取出这些牌中的最小值，从最小值开始连续取$groupSize$张，若能进行$n/groupSize$次，则可以组成连续的顺子。

```java
class Solution {
    public boolean isNStraightHand(int[] hand, int groupSize) {
        int n = hand.length;
        if(n % groupSize != 0) {
            return false;
        }
        int groups = n / groupSize;
        TreeMap<Integer, Integer> map = new TreeMap<>();
        for(int num : hand) {
            map.put(num, map.getOrDefault(num, 0) + 1);
            if(map.get(num) > groups) {
                return false;
            }
        }
        for(int i = 0; i < groups; i++) {
            int min = map.firstKey();
            for(int j = min; j < min + groupSize; j++) {
                if(map.get(j) != null) {
                    map.put(j, map.get(j) - 1);
                    if(map.get(j) == 0) {
                        map.remove(j);
                    }
                } else {
                    return false;
                }
            }
        }
        return true;
    }
}
```

上述过程是使用的$TreeMap$来计数的，其实也可以使用堆来维护每找这组顺子的起点。

当剩余次数不足以组成一组顺子时，则返回$false$即可，否则把这组顺子内的元素出现次数进行$-1$即可。

并且，这里如果当遍历到的最小值次数为0时，也无需将其$remove$，只需跳过即可。

```java
class Solution {
    public boolean isNStraightHand(int[] hand, int groupSize) {
        int n = hand.length, groups = n / groupSize;
        if(n == 0 || n % groupSize != 0) {
            return false;
        }
        Queue<Integer> pq = new PriorityQueue<>((a, b) -> a - b);
        Map<Integer, Integer> map = new HashMap<>();
        for(int h : hand) {
            pq.offer(h);
            map.put(h, map.getOrDefault(h, 0) + 1);
        }
        while(!pq.isEmpty()) {
            int min = pq.poll();
            if(map.get(min) == 0) {
                continue;
            }
            for(int i = min; i < min + groupSize; i++) {
                if(map.get(i) != null) {
                    map.put(i, map.get(i) - 1);
                } else {
                    return false;
                }
            }
        }
        return true;
    }
}
```

