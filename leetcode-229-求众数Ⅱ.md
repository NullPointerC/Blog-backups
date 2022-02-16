---
title: leetcode-229-求众数Ⅱ
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: a5808e22
date: 2021-10-22 10:51:16
---

[$link$](https://leetcode-cn.com/problems/majority-element-ii/)

![image-20211022105149127](https://gitee.com/cao_ziqiang/img/raw/master/20211022105149.png)

<hr/>

一个比较朴素的想法就是使用哈希表，用哈希存储$num->cnt$的关系。

```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        int n = nums.length;
        Map<Integer, Integer> map = new HashMap<>();
        for(int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        List<Integer> ans = new ArrayList<>();
        for(Integer key : map.keySet()) {
            if(map.get(key) > (n / 3)) {
                ans.add(key);
            }
        }
        return ans;
    }
}
```

此外也可以这么考虑，因为题目提到的众数需要出现的次数大于$[n/3]$。所以最多也就只有2个这样的众数，此时可以考虑使用摩尔投票法。

首先推出两个候选人$common1$以及$common2$，和他们出现的频次$cnt1$和$cnt2$。

若出现了和候选人一样的数字，则频次依次加1，若出现了不一样的，则减少频次。

若频次归0，则需要重新选举候选人，选举当前元素作为候选人。

```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        List<Integer> ans = new ArrayList<>();
        int n = nums.length;
        // 因为要超过n / 3,所以最多也就2个元素
        int common1 = nums[0], cnt1 = 0;
        int common2 = nums[0], cnt2 = 0;
        for(int num : nums) {
            if(cnt1 > 0 && num == common1) {
                cnt1++;
            }else if(cnt2 > 0 && num == common2) {
                cnt2++;
            }else if (cnt1 == 0) {
                common1 = num;
                cnt1++;
            }else if (cnt2 == 0) {
                common2 = num;
                cnt2++;
            }else{
                cnt1--;
                cnt2--;
            }
        }
        cnt1 = cnt2 = 0;
        for(int num : nums) {
            if(num == common1) {
                cnt1++;
            }else if(num == common2) {
                cnt2++;
            }
        }
        if(cnt1 > n / 3) {
            ans.add(common1);
        }
        if(cnt2 > n / 3) {
            ans.add(common2);
        }
        return ans;
    }
}
```

