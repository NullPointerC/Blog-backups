---
title: leetcode-638-大礼包
date: 2021-10-24 10:24:19
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
---

[$link$](https://gitee.com/cao_ziqiang/img/raw/master/20211026102608.png)

![image-20211026102607788](https://gitee.com/cao_ziqiang/img/raw/master/20211026102608.png)

<hr/>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211026102643.jpg)

各位程序猿1024快乐呀,希望少些说变就变的需求,少些996。

<hr/>

今天的题目还是有难度的，想了挺久的回溯整体思路是对的，但是在调试的代码的时候花了几个小时才搞定了。

- 我们考虑对购买进行回溯购买，回溯结束的条件为不需要购买了，即$\forall(x) \rightarrow x = 0(x \in needs)$
- 在购买时优先考虑使用大礼包购买,如果大礼包的某一物品数量大于所需要的数量,则不进行购买,当且仅当所需数小于大礼包的物品数量时才购买
- 先尝试所有的物品都在大礼包中购买,购买完毕后再去单买

```java
class Solution {
    private int ans = Integer.MAX_VALUE;
    public int shoppingOffers(List<Integer> price, List<List<Integer>> special, List<Integer> needs) {
        // 全部以单价购买
        int cost = 0;
        for (int i = 0; i < needs.size(); i++) {
            cost += (price.get(i) * needs.get(i));
        }
        ans = Math.min(ans, cost);
        // 搜索
        dfs(price, special, needs, 0);
        return ans;
    }
    private void dfs(List<Integer> price, List<List<Integer>> special, List<Integer> needs, int cost) {
        boolean flag = true;
        for(int need : needs) {
            if(need != 0) {
                flag = false;
                break;
            }
        }
        if (flag) {
            ans = Math.min(ans, cost);
            return;
        }
        for(List<Integer> list : special) {
            flag = true;
            for(int i = 0; i < list.size() - 1; i++) {
                if(list.get(i) > needs.get(i)) {
                    flag = false;
                    break;
                }
            }
            if (flag) {
                for(int i = 0; i < needs.size(); i++) {
                    needs.set(i, needs.get(i) - list.get(i));
                }
                dfs(price, special, needs, cost + list.get(list.size() - 1));
                for (int i = 0; i < needs.size(); i++) {
                    needs.set(i, needs.get(i) + list.get(i));
                }
            }
        }
        for (int i = 0; i < needs.size(); i++) {
                cost += (needs.get(i) * price.get(i));
            }
        ans = Math.min(ans, cost);  
    }
}
```

