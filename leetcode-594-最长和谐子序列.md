---
title: leetcode-594-最长和谐子序列
date: 2021-11-20 10:33:38
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/longest-harmonious-subsequence/)

<hr/>

![image-20211120103433975](https://gitee.com/cao_ziqiang/img/raw/master/20211120103434.png)

<hr/>

看到和谐子序列的定义就可以知道这个子序列中只包含有2个相邻的元素,所以我们可以先将数组排序,这样相邻元素就在一起了,因为这里只需要最终的长度,不要求写出子序列具体元素是哪些，所以可以排序。

排好序后，再使用双指针，固定一个$i$指针指向$num$，而使$j$往后遍历，直到$j$指针指向的元素不为$num+1$。此时计算这个窗口的大小。

```java
class Solution {
    public int findLHS(int[] nums) {
        Arrays.sort(nums);
        int i = 0, j = 0, ans = 0, n = nums.length;
        while (i < n) {
            j = i + 1;
            while(j < n) {
                if (nums[j] == nums[i]) {
                    ++j;
                    continue;
                } else if (nums[j] == nums[i] + 1){
                    ++j;
                    ans = Math.max(ans, j - i);
                    continue;
                } else {
                    break;
                }
            }
            i++;
        }
        return ans;
    }
}
```

上述代码也可以做一点小优化，即，只在$nums[j]-nums[i]=1$时判$ans$。等于0时，只移动$j$即可，大于则同时移动两个指针，减少不必要的$i$指针移动。

```java
class Solution {
    public int findLHS(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length, ans = 0;
        for (int i = 0, j = 0; j < n; j++) {
            while (i < j && nums[j] - nums[i] > 1) i++;
            if (nums[j] - nums[i] == 1) ans = Math.max(ans, j - i + 1);
        }
        return ans;
    }
}
```

也可以使用$map$来存储每一个元素的出现次数。我们要求的就是每个$value$以及$value+1$出现的次数之和。

```java
class Solution {
    private  Map<Integer, Integer> map = new HashMap<>();
    public int findLHS(int[] nums) {
        int ans = 0;
        for(int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        // System.out.println(map);
        for(Integer key : map.keySet()) {
            if (map.containsKey(key + 1)) {
                ans = Math.max(ans, map.get(key) + map.get(key + 1));
            }
        }
        return ans;
    }
}
```

