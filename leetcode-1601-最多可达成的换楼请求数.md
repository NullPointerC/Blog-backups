---
title: leetcode-1601-最多可达成的换楼请求数
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 9aedc123
date: 2022-02-28 23:10:32
---

[1601. 最多可达成的换楼请求数目](https://leetcode-cn.com/problems/maximum-number-of-achievable-transfer-requests/)

![image-20220228231108854](https://gitee.com/cao_ziqiang/img/raw/master/20220228231108.png)
<hr/>
![img](https://gitee.com/cao_ziqiang/img/raw/master/20220301120841.jpeg)
<hr/>
![image-20220301120852989](https://gitee.com/cao_ziqiang/img/raw/master/20220301120853.png)

<hr/>

因为数据范围比较小，我们可以尝试枚举所有的`requests`请求的子集，求得所有子集中满足`request`的最大集合数。

我们可以发现，`int[]`中`from`表示出度，`to`表示入度，只需使所有点的入度和出度都相等，这就是一种满足的情况。

我们可以用`inDegree`和`outDegree`表示每个点的入度和出度，二者相等时，即一种可行解，并维护一个子集`subset`，最大可满足子集就是题目要求的可行列表中最大请求数目。

```java
class Solution {
    int res = Integer.MIN_VALUE;
    List<int[]> subset = new ArrayList<>();
    public int maximumRequests(int n, int[][] requests) {
        int l = requests.length;
        int[] inDegree = new int[n];
        int[] outDegree = new int[n];
        dfs(requests, 0, l, inDegree, outDegree);
        return res;
    }

    public void dfs(int[][] requests, int idx, int end, int[] inDegree, int[] outDegree) {
        if(Arrays.equals(inDegree, outDegree)) {
            res = Math.max(res, subset.size());
        }
        if(idx >= end) {
            return ;   
        }
        for(int i = idx; i < end; i++) {
            subset.add(requests[i]);
            inDegree[requests[i][1]]++;
            outDegree[requests[i][0]]++;
            dfs(requests, i + 1, end, inDegree, outDegree);
            outDegree[requests[i][0]]--;
            inDegree[requests[i][1]]--;
            subset.remove(subset.size() - 1);
        }
    }
}
```

