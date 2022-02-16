---
title: leetcode-911-在线选举
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 8b3e34a5
date: 2021-12-11 22:37:00
---

[在线选举](https://leetcode-cn.com/problems/online-election/)

<hr/>

![image-20211211223749275](https://gitee.com/cao_ziqiang/img/raw/master/20211211223749.png)

<hr/>

tags:(二分、哈希表)

这里我们可以使用一个数组来存$times[i]$时刻的最高得票的候选人编号。

只需要将之前获得的最高票数的候选人的的得票数和当前的人的得票数比较即可，在这一步维护最高得票数的候选人$id$。

这一步并不难实现，在$q$方法内要实现查询就需要注意题目给到的条件：

![image-20211211224047693](https://gitee.com/cao_ziqiang/img/raw/master/20211211224047.png)

$times$是一个严格递增的有序数组，所以可以不用$O(n)$的遍历找$t$在$times$中的左边界，可以二分找$t$的左边界。

```java
class TopVotedCandidate {
    // 表示times[i]时的最高得票数
    private int[] timesTop;
    private Map<Integer, Integer> personVotes;
    private int[] _times;
    public TopVotedCandidate(int[] persons, int[] times) {
        int n = persons.length;
        this._times = times;
        // int top = Integer.MIN_VALUE;
        int topPerson = -1;
        timesTop = new int[n];
        personVotes = new HashMap<>();
        for(int i = 0; i < n; i++) {
            if(!personVotes.containsKey(persons[i])) {
                personVotes.put(persons[i], personVotes.getOrDefault(persons[i], 0) + 1);
            } else {
                personVotes.put(persons[i], personVotes.get(persons[i]) + 1);
            }
            if(personVotes.get(persons[i]) >= personVotes.getOrDefault(topPerson, 0)) {
                topPerson = persons[i];
            }
            timesTop[i] = topPerson;
        }
        // System.out.println(Arrays.toString(timesTop));
    }
    
    public int q(int t) {
        // 在times数组中二分找第一个小于等于t的时间
        // System.out.println(Arrays.toString(_times));
        int left = 0, right = _times.length - 1;
        int mid = 0;
        if (t > _times[right]) {
            return timesTop[right];
        }
        while(left < right) {
            mid = (left + right + 1) >> 1;
            if (_times[mid] > t) {
                right = mid - 1;
            } else {
                left = mid;
            }
        }
        return timesTop[right];
    }
}

/**
 * Your TopVotedCandidate object will be instantiated and called as such:
 * TopVotedCandidate obj = new TopVotedCandidate(persons, times);
 * int param_1 = obj.q(t);
 */
```

在评论区还看有有用$TreeMap$的，使代码简洁了不少。

```java
class TopVotedCandidate {
    // 人获得的票数
    Map<Integer,Integer> cntMap = new HashMap<>();
    // 当前时间领先的人
    TreeMap<Integer,Integer> tm = new TreeMap<>();
    public TopVotedCandidate(int[] persons, int[] times) {
        int maxPersonId = -1;
        for(int i = 0;i < persons.length;i++) {
            // 人的得票增加
            int cnt = cntMap.getOrDefault(persons[i],0) + 1;
            cntMap.put(persons[i],cnt);
            // 之前得票最多的人
            int maxCnt = cntMap.getOrDefault(maxPersonId,0);
            if(cnt >= maxCnt) {
                maxPersonId = persons[i];
            }
            // 维护当前时间得票最多的personId
            tm.put(times[i],maxPersonId);
        }
    }
    
    public int q(int t) {
        return tm.floorEntry(t).getValue();
    }
}
```

