---
title: leetcode-2016-增量元素之间的最大差值
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: d5324083
date: 2022-02-26 12:28:38
---

[2016. 增量元素之间的最大差值](https://leetcode-cn.com/problems/maximum-difference-between-increasing-elements/)

![image-20220226122922189](https://gitee.com/cao_ziqiang/img/raw/master/20220226122922.png)

<hr/>

比较暴力的做法就是`O(N^2)`地枚举每一组`nums[i]`he`nums[j]`。

```java
class Solution {
    public int maximumDifference(int[] nums) {
        int n = nums.length;
        int res = -1;
        for(int i = 0; i < n; i++) {
            for(int j = i + 1; j < n; j++) {
                if(nums[j] > nums[i]) res = Math.max(res, nums[j] - nums[i]);
            }
        }
        return res;
    }
}
```

也可以考虑从数组尾部开始往前构造单调递减栈，如果需要出栈时，此时依次比较栈顶与栈底的差。

```java
class Solution {
    public int maximumDifference(int[] nums) {
        int n = nums.length;
        Deque<Integer> st = new ArrayDeque<>();
        int res = -1;
        st.push(nums[n - 1]);
        int bottom = st.peek();
        for(int i = n - 2; i >= 0; i--) {
            while(!st.isEmpty() && nums[i] > st.peek()) {
                if(st.size() > 1) {
                    res = Math.max(res, bottom - st.peek());
                }
                st.pop();
            }
            st.push(nums[i]);
            if(st.size() == 1) {
                bottom = nums[i];
            }
        }
        if(st.size() > 1) {
            res = Math.max(res, bottom - st.peek());
        }
        return res == 0 ? -1 : res;
    }
}
```

其实经过观察可以发现，这题就是卖股票Ⅰ的变种，只需要维护最小即可。

```cpp
class Solution {
    public int maximumDifference(int[] nums) {
        int n = nums.length;
        int ans = -1, premin = nums[0];
        for (int i = 1; i < n; ++i) {
            if (nums[i] > premin) {
                ans = Math.max(ans, nums[i] - premin);
            } else {
                premin = nums[i];
            }
        }
        return ans;
    }
}
```

