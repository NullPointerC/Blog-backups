---
title: leetcode-373-查找和最小的K对数字
date: 2022-01-14 22:58:44
categories: LeetCode
tags: [LeetCode,algorithm]
---

[373. 查找和最小的 K 对数字](https://leetcode-cn.com/problems/find-k-pairs-with-smallest-sums/)

<hr/>

![image-20220114225928944](https://gitee.com/cao_ziqiang/img/raw/master/20220114225929.png)

这里一个比较朴素的做法就是直接两重循环找到这两个数组中的所有可能和的组合$pair$,将其存储进一个队列中,然后取出这个队列的头$k$组即可。

```java
class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        List<List<Integer>> ans = new ArrayList<>();
        int n1 = nums1.length, n2 = nums2.length;
        Queue<Pair> q = new PriorityQueue<>((p1, p2) -> {return (p1.x + p1.y) - (p2.x + p2.y);});
        for(int i = 0; i < n1; i++) {
            for(int j = 0; j < n2; j++) {
                q.offer(new Pair(nums1[i], nums2[j]));
            }
        }
        while(!q.isEmpty() && ans.size() < k) {
            List<Integer> temp = new ArrayList<>();
            Pair pair = q.poll();
            // System.out.println(pair);
            temp.add(pair.x);
            temp.add(pair.y);
            ans.add(temp);
        }
        return ans;
    }
    private static class Pair {
        public int x;
        public int y;

        public Pair() {}

        public Pair(int x, int y) {
            this.x = x;
            this.y = y;
        }
        
        @Override
        public String toString() {
            return "(" + this.x + "," + this.y + ")";
        }
    }
}
```

但是我们观察数据范围是$10^5$，双重循环嵌套之下就是$10^{10}$，必然会超时或者空间溢出。所以考虑时间复杂度小于$O(n^2)$的算法。

![image-20220114230748492](https://gitee.com/cao_ziqiang/img/raw/master/20220114230748.png)

于是考虑怎么样优化空间的浪费。这里其实提到，$nums1$和$nums2$都是已经排序后的数组，因此我们可以考虑，存储下每一对数对中两个元素对应的在每个数组中的下标，这样

```java
class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        List<List<Integer>> ans = new ArrayList<>();
        Queue<int[]> q = new PriorityQueue<>((a, b) -> {return (nums1[a[0]] + nums2[a[1]]) - (nums1[b[0]] + nums2[b[1]]);});
        int n1 = nums1.length, n2 = nums2.length;
        for(int i = 0; i < Math.min(n1, k); i++) {
            q.offer(new int[]{i, 0});
        }
        while(ans.size() < k && !q.isEmpty()) {
            int[] idx = q.poll();
            ans.add(List.of(nums1[idx[0]], nums2[idx[1]]));
            if(idx[1] + 1 < n2) {
                q.offer(new int[]{idx[0], idx[1] + 1});
            }
        }
        return ans;
    }
}
```

因为需要插入$k$组数对，且$nums1$和$nums2$中的数字必然是从小的开始，这里假定在只取$k$组数对满足时，每个从$nums1$中取的数从$i$开始，每个从$nums2$中取的数从$0$开始。且每取完一对，就需要将这一对元素的下一个位置加入堆，以确保有更小的不被漏掉。



