---
title: leetcode-228-汇总区间
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: b78a85ac
date: 2021-06-22 13:42:15
---

<a href="https://leetcode-cn.com/problems/summary-ranges/">传送门</a>

<hr/>

题目描述:

<pre>
    给定一个无重复元素的有序整数数组 nums 。
    返回 恰好覆盖数组中所有数字 的 最小有序 区间范围列表。也就是说，nums 的每个元素都恰好被某个区间范围所覆
    盖，并且不存在属于某个范围但不属于 nums 的数字 x 。
    列表中的每个区间范围 [a,b] 应该按如下格式输出：
	"a->b" ，如果 a != b
	"a" ，如果 a == b
</pre>

<hr/>

示例1:

<pre>
    输入：nums = [0,1,2,4,5,7]
	输出：["0->2","4->5","7"]
	解释：区间范围是：
	[0,2] --> "0->2"
	[4,5] --> "4->5"
	[7,7] --> "7"
</pre>

示例2:

<pre>
    输入：nums = [0,2,3,4,6,8,9]
	输出：["0","2->4","6","8->9"]
	解释：区间范围是：
	[0,0] --> "0"
	[2,4] --> "2->4"
	[6,6] --> "6"
	[8,9] --> "8->9"
</pre>

示例3:

<pre>
    输入：nums = []
	输出：[]
</pre>

示例4:

<pre>
    输入：nums = [-1]
	输出：["-1"]
</pre>

示例5:

<pre>
    输入：nums = [0]
	输出：["0"]
</pre>

<hr/>

提示:

<pre>
    0 <= nums.length <= 20
	-231 <= nums[i] <= 231 - 1
	nums 中的所有值都 互不相同
	nums 按升序排列
</pre>

<hr/>

思路:

<pre>
    我们需要知道题目说的覆盖是什么意思,即nums的每个元素都在区间内,并且不存在有该区间的数字不在nums中
    并且该数组是一个有序数组,那么相邻大小的数字则必然是处于相邻位置,所以我们只需要设置双指针即可
    慢指针设置为区间起始索引,快指针则依次比较,比较到不属于该区间的数字则退出内层循环,并且将该区间加入list中
    内层循环完成之后,将慢指针前移到区间的下一个位置
</pre>

代码:

```java
class Solution {
    public List<String> summaryRanges(int[] nums) {
        int l = nums.length;
        List<String> strs = new LinkedList<>();
        /*处理边界情况*/
        if (l == 0) {
            return strs;
        }

        /*处理边界情况*/
        if(l == 1) {
            strs.add(String.valueOf(nums[l-1]));
            return strs;
        }
        int i = 0;
        while(i < l) {
            int j;
            for(j = i; j < l - 1; j++) {
                if(nums[j+1]-nums[j] == 1) {
                    continue;
                }else {
                    break;
                }
            }
            StringBuilder sb = new StringBuilder();
            if(i != j) {
                sb.append(String.valueOf(nums[i]));
                sb.append("->");
                sb.append(String.valueOf(nums[j]));
            }else {
                sb.append(String.valueOf(nums[i]));
            }
            strs.add(sb.toString());
            i = j + 1;
        }
        return strs;
    }
}
```

![image-20210622135152918](https://gitee.com/cao_ziqiang/img/raw/master/20210622135153.png)

