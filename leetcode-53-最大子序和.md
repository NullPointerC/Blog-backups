---
title: leetcode-53-最大子序和
date: 2021-07-02 17:26:50
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/maximum-subarray/)

<hr/>

题目描述:

<pre>
给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。
</pre>

示例1:

<pre>
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
</pre>

示例2:

<pre>
输入：nums = [1]
输出：1
</pre>

示例3:

<pre>
输入：nums = [0]
输出：0
</pre>

示例4:

<pre>
输入：nums = [-1]
输出：-1
</pre>

示例5:

<pre>
输入：nums = [-100000]
输出：-100000
</pre>

思路:

一般题目设计最大和子数组这样的字眼的,往往都是用动态规划(dp)来解决

dp[i]代表遍历到数组i位置时的最大值,这是我们就看到了i位置后,就需要比较当前元素,前i-1个位置+当前元素

哪个大,取更大的那个,如果前边累加后还不如自己本身大，那就把前边的都扔掉，从此自己本身重新开始累加。并且看到了后面的最大值会不会比前一步的大,如果大则替换掉,否则继续遍历看是否会更大

代码:

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int l = nums.length;
        if(l == 0 || null == nums) {
            return 0;
        }
        int[] dp = new int[l];
        dp[0] = nums[0];
        int ret = dp[0];
        for(int i = 1; i < l; i++) {
            /*动态规划： 状态转移方程：dp[i] = max(dp[i-1] + nums[i], nums[i])*/
            dp[i] = Math.max(nums[i],dp[i - 1]+nums[i]);
            /*dp[i]表示nums中以nums[i]结尾的最大子序和*/
            /*dp[i]=max(dp[i-1]+nums[i],nums[i])*/
			/*dp[i]时当前数字,要么是与前面的最大子序和的和*/
            ret = Math.max(dp[i],ret);
        }
        return ret;
    }
}
```

![image-20210702174154527](https://gitee.com/cao_ziqiang/img/raw/master/20210702174154.png)

这里可以再优化一点空间,dp数组前面存储的只是为了方便我们记忆化,我们可以不使用数组来做记忆化,用一个变量就可以

优化版本如下:

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int pre = 0, maxAns = nums[0];
        for (int x : nums) {
            pre = Math.max(pre + x, x);
            maxAns = Math.max(maxAns, pre);
        }
        return maxAns;
    }
}
```

<hr/>

以下两个版本为评论区中所学

也可以使用**`贪心`**的思想,从左向右迭代,一个个数字加过去,如果sum<0,重新开始找子序串

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int l = nums.length;
        if(l == 0 || null == nums) {
            return 0;
        }
        int maxAns = Integer.MIN_VALUE;
        int sum = 0;
        for(int i = 0; i < l; i++) {
            /*sum要一直加数组的第i项,模拟数组的子序列过程*/
            sum += nums[i];
            /*看此时sum加上这一个数组项后会不会更大,更大则替换之*/
            maxAns = Math.max(maxAns, sum);
            if(sum < 0) {
                /*如果sum小于0了,继续加也是小于0的,得不到最大的子序列和,就设sum为0*/
                sum = 0;
            }
        }
        return maxAns;
    }
}
```

这题还可以用**`线段树+分治`**来求解,后面去查了一下[线段树](https://www.jianshu.com/p/6fd130084a43),线段树是专门用来处理数组区间查询和更新操作的数据结构,并且可以用来求区间最大最小值,进行更新和查询的时间复杂度可以优化到O(lgn)级

线段树的关键在于使用一棵二叉树来存储对应每一个区间(segment)的数据。同时,这个二叉树也是使用的一个数组来存储的

例如，给定一个长度为<font color="blue">`N`</font>的数组<font color="blue">`arr`</font>，其所对应的线段树T各个结点的含义如下：

1. <font color="blue">`T`</font>的根结点代表整个数组所在的区间对应的信息，及<font color="blue">`arr[0:N]`</font>（不含N)所对应的信息。

2. <font color="blue">`T`</font>的每一个叶结点存储对应于输入数组的每一个单个元素构成的区间<font color="blue">`arr[i]`</font>所对应的信息，此处0≤i<N。

3. <font color="blue">`T`</font>的每一个中间结点存储对应于输入数组某一区间<font color="blue">`arr[i:j]`</font>对应的信息，此处0≤i<j<N。

以根结点为例，根结点代表<font color="blue">`arr[0:N]`</font>区间所对应的信息，接着根结点被分为两个子树，分别存储<font color="blue">`arr[0:(N-1)/2]`</font>及<font color="blue">`arr[(N-1)/2+1:N]`</font>两个子区间对应的信息。也就是说，对于每一个结点，其左右子结点分别存储母结点区间拆分为两半之后各自区间的信息。也就是说对于长度为N的输入数组，线段树的高度为<font color="blue">`logN`</font>。

对于一个线段树来说，其应该支持的两种操作为：

1. Update：更新输入数组中的某一个元素并对线段树做相应的改变。
2. Query：用来查询某一区间对应的信息（如最大值，最小值，区间和等）。



我们定义一个操作get(nums,l,r)来查询nums序列的[l,r]区间的最大字序列和,那么我们本题的最终答案就是get(nums,0,nums.length-1)

本题代码如下:

```java
class Solution {
    public int maxSubArray(int[] nums) {
        return new Status().get(nums,0,nums.length-1).mMax;
    }
    private class Status {
        /*
        * lMax代表区间[l,r]上，以l为起点的最大子序列
        * rMax代表区间[l,r]上，以r为终点的最大子序列
        * mMax代表区间[l,r]上的最大子序列
        * aMax代表区间[l,r]的序列和，主要是为了l.iSum+r.lSum，过中点m的最大子序列
        * */
        int lMax,rMax,mMax,aMax;

        public Status() {
        }

        public Status(int lMax, int rMax, int mMax, int aMax) {
            this.lMax = lMax;
            this.rMax = rMax;
            this.mMax = mMax;
            this.aMax = aMax;
        }

        public Status pushUp(Status lStatus, Status rStatus) {
            /*lMax有可能等于左区间的lMax,也可能等于左区间的和加上右区间的lMax,二者取大*/
            int lMax = Math.max(lStatus.lMax, lStatus.aMax + rStatus.lMax);
            /*rMax有可能等于右区间的rMax,也可能等于右区间的和加上左区间的rMax,二者取大*/
            int rMax = Math.max(rStatus.rMax, lStatus.rMax + rStatus.aMax);
            /*mMax有可能在两个区间中的一个mMax中取得,也可能由两个区间组合而取得,故三者取大*/
            int mMax = Math.max(lStatus.rMax + rStatus.lMax, Math.max(lStatus.mMax, rStatus.mMax));
            /*[l,r]区间和就等于左区间和加上右区间和*/
            int aMax = lStatus.aMax + rStatus.aMax;
            return new Status(lMax,rMax,mMax,aMax);
        }

        /**
         * 查询 nums序列 [l,r]区间内的最大子段和
         * @param nums 数组序列
         * @param l 区间左端点
         * @param r 区间右端点
         * @return 最大子段和
         */
        public Status get(int[] nums, int l ,int r) {
            if(l == r) {
                /*当递归逐层深入直到区间长度缩小为 1 的时候，递归「开始回升」*/
                return new Status(nums[l], nums[l], nums[l], nums[l]);
            }
            /*对于一个区间[l,r],我们取mid = (l+r)/2,对区间[l,mid],[mid+1,r]分治求解*/
            int mid = (l + r) >> 1;
            Status lStatus = get(nums,l,mid);
            Status rStatus = get(nums,mid+1,r);
            return pushUp(lStatus,rStatus);
        }
    }
}
```

![image-20210702211859786](https://gitee.com/cao_ziqiang/img/raw/master/20210702211859.png)

一晚上就投入进去了,不过真的很开心