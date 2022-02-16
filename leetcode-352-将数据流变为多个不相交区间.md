---
title: leetcode-352-将数据流变为多个不相交区间
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 79dcb7f0
date: 2021-10-09 20:35:20
---

[$link$](https://leetcode-cn.com/problems/data-stream-as-disjoint-intervals/solution/)

<hr/>

![image-20211009203610078](https://gitee.com/cao_ziqiang/img/raw/master/20211009203610.png)

![image-20211009203624400](https://gitee.com/cao_ziqiang/img/raw/master/20211009203624.png)

<hr/>

一个比较好想到的做法是，维护一个$list$。每次新增元素时就将元素添加进$list$中，然后要返回$interval$的时候先对$list$排序，再将其每个连续的片段找出来，连续的条件即为

```java
list.get(i) - list.get(i - 1) <= 1
```

如遇到第一个不连续的情况，则说明这是一个单独的不相交区间，将这个区间的首尾分别添加。

维护一个$front$头部，一直到不连续的尾部。并更新头部为尾部的下一个位置。

```java
class SummaryRanges {
    List<Integer> list;
    public SummaryRanges() {
        list = new ArrayList<>();
    }
    
    public void addNum(int val) {
        list.add(val);
    }
    
    public int[][] getIntervals() {
        if(list.size() == 1) {
            return new int[][]{{list.get(0), list.get(0)}};
        }
        // 先排序
        Collections.sort(list);
        List<int[]> intervals = new ArrayList<>();
        // 维护头部
        int front = list.get(0);
        for(int i = 1; i < list.size(); i++) {
            // 碰到了不连续的条件
            if(list.get(i) - list.get(i - 1) > 1) {
                intervals.add(new int[]{front, list.get(i - 1)});
                // 更新头部
                front = list.get(i);
            }
        }
        intervals.add(new int[]{front, list.get(list.size() - 1)});
        return intervals.toArray(new int[intervals.size()][]);
    }
}

/**
 * Your SummaryRanges object will be instantiated and called as such:
 * SummaryRanges obj = new SummaryRanges();
 * obj.addNum(val);
 * int[][] param_2 = obj.getIntervals();
 */
```

<hr/>

也可以先使用$TreeSet$在插入时就排好序，要返回$intervals$时再去找区间。

```java
class SummaryRanges {
    
    TreeSet<Integer> set;
    public SummaryRanges() {
        set = new TreeSet<>(); 
    }
    
    public void addNum(int val) {
        set.add(val);
    }
    
    public int[][] getIntervals() {
        List<int []> list = new ArrayList<>();
        Iterator<Integer> it = set.iterator();
        int left = -1;
        int right = -1;
        while (it.hasNext()) {
            int v = it.next();
            if (left == -1) {
                left = right = v;
            } else {
                if (right + 1 == v) {
                    right = v;
                } else {
                    list.add(new int[]{left, right});
                    left = right = v;
                }
            }
        }
        list.add(new int[]{left, right});
        return list.toArray(new int[list.size()][]);
    }
}
```

