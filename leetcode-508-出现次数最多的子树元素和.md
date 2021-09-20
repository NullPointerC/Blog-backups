---
title: leetcode-508-出现次数最多的子树元素和
date: 2021-09-19 18:13:59
categories: LeetCode
tags: [algorithm,Java,LeetCode]

---

[$link$](https://leetcode-cn.com/problems/most-frequent-subtree-sum/)

<hr/>

![image-20210919181449906](https://gitee.com/cao_ziqiang/img/raw/master/20210919181449.png)

<hr/>

这里首先是用了一种很低效的解法，定义一个求结点子树元素和的函数，并对树进行深搜，深搜时将子树元素和$sum$和它出现的频率更新进$map$中，并且记录下$maxFreq$，即出现次数最多的子树元素和的频率。

最后对每个$key$进行遍历即可。

```java
class Solution {
    
    private Map<Integer, Integer> sumCount = new HashMap<>();
    private Integer maxFrequent = Integer.MIN_VALUE;
    
    public int[] findFrequentTreeSum(TreeNode root) {
        
        postOrder(root);
        
        List<Integer> help = new ArrayList<>();
        
        // 将符合条件的所有元素和添加到 help list 当中
        for (Map.Entry<Integer, Integer> next : sumCount.entrySet()) {
            
            // 添加计数等于最大次数 maxFrequent 的 元素和
            if (next.getValue() == maxFrequent) {
                help.add(next.getKey());
            }
        }
        
        int[] res = new int[help.size()];
        
        for (int i = 0; i < help.size(); i++) {
            res[i] = help.get(i);
        }
        
        return res;
    }
    
    private int postOrder(TreeNode node) {
        
        // 当节点为 null, 它的元素和等于 0， 利于它的父节点进行元素和统计
        if (node == null) {
            return 0;
        }
        
        int left = postOrder(node.left);
        int right = postOrder(node.right);
        
        int sum = left + right + node.val;
        
        int value = sumCount.getOrDefault(sum, 0);
        
        sumCount.put(sum, value + 1);
        
        maxFrequent = Math.max(maxFrequent, value + 1);
        
        return sum;
        
    }
}
```

