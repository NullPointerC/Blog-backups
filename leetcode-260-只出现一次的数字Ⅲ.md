---
title: leetcode-260-只出现一次的数字Ⅲ
categories:
  - LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: a4ce05c6
date: 2021-10-30 21:20:02
---

[$link$](https://leetcode-cn.com/problems/single-number-iii/)

教资坐牢。。。坐牢结束就来写题了。

<hr/>

![image-20211030212058784](https://gitee.com/cao_ziqiang/img/raw/master/20211030212058.png)

<hr/>

一个比较朴素的解法就是直接使用$set$，如果$add$返回$false$，说明集合中已经存在，那么我们将其从集合中剔除，在最后将集合中剩余元素返回即是只出现了1次的元素。

```java
class Solution {
    public int[] singleNumber(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for(int num : nums) {
            if(!set.add(num)) {
                set.remove(num);
            }
        }
        List<Integer> ansList = new ArrayList<Integer>(set);
        int[] ans = new int[ansList.size()];
        for(int i = 0; i < ansList.size(); i++) {
            ans[i] = ansList.get(i);
        }
        return ans;
    }
}
```

想到了先用异或把相同的全部消除掉，然后就不知道怎么做了，没有想过分类，这下学到了。

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211030215306.jpg)

```java
class Solution {
    public int[] singleNumber(int[] nums) {
        int xor = 0;
        for(int num : nums) {
            xor ^= num;
        }
        int diff = xor & (~xor + 1);
        int a = 0, b = 0;
        for(int num : nums) {
            if((num & diff) != 0) {
                a ^= num;
            }else {
                b ^= num;
            }
        } 
        return new int[]{a, b};
    }
}
```

这时候举个栗子,比如我们数组中只出现1次的数字分别是$a$和$b$,这时我们考虑,异或一趟数组得到的结果就是$\large a \large \text{^}b$。

考虑得到这个结果的最低的那位$1$，因为如果出现了$1$，说明$a$和$b$中的这1位有1个为1，另一个为0。

所以考虑再遍历一趟数组，如果这位也是1的，想与后结果不为0，所以这两个答案将会被分到不一样的组中，最后可以得到$a$和$b$的值。

```java
class Solution {
    public int[] singleNumber(int[] nums) {
        int xor = 0;
        for(int num : nums) {
            xor ^= num;
        }
        int diff = xor & (~xor + 1);
        int a = 0, b = 0;
        for(int num : nums) {
            if((num & diff) != 0) {
                a ^= num;
            }else {
                b ^= num;
            }
        } 
        return new int[]{a, b};
    }
}
```

