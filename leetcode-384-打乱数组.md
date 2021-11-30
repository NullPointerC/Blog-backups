---
title: leetcode-384-打乱数组
date: 2021-11-22 10:48:27
categories: LeetCode
tags: [LeetCode,algorithm]
---

[打乱数组](https://leetcode-cn.com/problems/shuffle-an-array/)

<hr/>

![image-20211122105155409](https://gitee.com/cao_ziqiang/img/raw/master/20211122105155.png)

<hr/>

涉及到经典的洗牌算法，又学习了一种新的算法。

洗牌算法的核心在于等概率地返回所有可能的排列情况。

看到很多大佬的解释就是：在前面$n-1$张牌已经洗好的情况下，第$n$张牌和前面$n-1$张牌中的随机其中一个交换，或者不换。

其实光这么看，是比较难看懂的，举个栗子就更好理解了。

假设数组为$[2,5,3,1,4]$，从后往前遍历，假设第一轮我们选取的分界限是$n-1$号元素，那么它和前面$n-1$个元素交换，若交换为$[2,4,3,1,5]$，此时分界线再往前移动，到了$n-2$号元素，这样就不会和前面的重复，所以这样能够满足随机化的算法。

```java
class Solution {
    int [] nums;
    int [] shuffle;
    Random random;
    public Solution(int[] nums) {
        this.nums = nums;
        random = new Random();
    }
    
    public int[] reset() {
        return nums;
    }
    
    public int[] shuffle() {
        shuffle = nums.clone();
        for(int i = shuffle.length - 1; i >= 0; i--){
            int j = random.nextInt(i + 1);
            swap(i, j);
        }
        return shuffle;
    }
    void swap(int i, int j){
        int temp = shuffle[i];
        shuffle[i] = shuffle[j];
        shuffle[j] = temp;
    }
}
```









