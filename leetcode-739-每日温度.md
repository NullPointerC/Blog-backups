---
title: leetcode-739-每日温度
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: dff54e6
date: 2022-02-23 11:12:26
---

## description

[739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

![image-20220223111338208](https://gitee.com/cao_ziqiang/img/raw/master/20220223111338.png)

比较典型的单调栈的类型，且这里的是递减栈，因为需要用后面的大数消除当前栈顶的小数。

换成本题的含义，也就是需要用后面较高的温度使当前栈中左边比它低温度的弹出栈。

下面介绍一下单调栈

## 单调栈

顾名思义，单调栈也就是栈中存放的数据是有序的，同时分为单调递增栈和单调递减栈。

下面举个递减栈的例子，有一组数

`10, 3, 7, 4, 12`，从左到右依次入栈，如果栈为空，或者入栈元素小于栈顶元素的值，那么元素入栈，否则需要把栈中比入栈元素小的元素全部出栈后，才可以入栈，否则会破坏栈的单调性。

首先栈为空，`10`入栈，栈内元素为`10`；

`3`入栈时，比栈顶元素`10`小，`3`入栈，栈内元素为`10, 3`；

`7`入栈时，比栈顶元素`3`大，需要先将比它小的元素弹出，直到栈顶为`10`，比`7`大，将`7`入栈，栈内元素为`10, 7`；

`4`入栈时，比栈顶元素小，直接入栈，栈内元素为`10, 7, 4`；

`12`入栈时，发现栈顶元素比它小，出栈，直到栈为空，`12`入栈，栈内元素为`12`；

以下是单调栈的伪代码

```cpp
stack<int> st;
//此处一般需要给数组最后添加结束标志符
for (遍历这个数组)
{
	if (栈空 || 栈顶元素大于等于当前比较元素)
	{
		入栈;
	}
	else
	{
		while (栈不为空 && 栈顶元素小于当前元素)
		{
			栈顶元素出栈;
			更新结果;
		}
		当前数据入栈;
	}
}
```

对于本题，就需要判断下一个更高的温度距离当前温度有几天即可。

```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        if(temperatures == null || temperatures.length == 0) {
            return new int[0];
        }
        int n = temperatures.length;
        int[] answer = new int[n];
        // 设置递减栈
        Stack<Integer> st = new Stack<>();
        for(int i = 0; i < n; i++) {
            int temperature = temperatures[i];
            // 找到了更高的温度
            while(!st.isEmpty() && temperatures[st.peek()] < temperature) {
                answer[st.peek()] = i - st.peek();
                st.pop();
            }
            st.push(i);
        }
        // 还留在栈中的都是没有更高温度了的,其实也可以省略,因为new出来的数组默认值就是0
        while(!st.isEmpty()) {
            answer[st.peek()] = 0;
            st.pop();
        }
        return answer;
    }
}
```

