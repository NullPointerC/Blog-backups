---
title: 算法小抄-4-子序列dp
date: 2021-09-08 11:05:40
categories: algorithm
tags: [计算机基础,backend,数据结构]

---

## LIS问题

LIS即最长递增子序列问题。

$dp$的难点在于寻找正确的状态转移方程。

在$dp$中比较通用的技巧就是使用数学归纳。

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210908112039.png)

在本题中利用数学归纳可以进行如下构建：

**`dp[i]` 表示以 `nums[i]` 这个数结尾的最长递增子序列的长度。**

根据这个定义，我们就可以推出 base case：`dp[i]` 初始值为 1，因为以 `nums[i]` 结尾的最长递增子序列起码要包含它自己。

算法演进的过程是这样的：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210908112319.gif)

在这个过程中，我们应该怎么设计算法逻辑来正确计算每个 `dp[i]` 呢？

这里就可以使用数学归纳的思想：

**假设我们已经知道了 `dp[0..4]` 的所有结果，我们可以通过这些已知结果推出 `dp[5]`** 

根据刚才我们对 `dp` 数组的定义，现在想求 `dp[5]` 的值，也就是想求以 `nums[5]` 为结尾的最长递增子序列。

**`nums[5] = 3`，既然是递增子序列，我们只要找到前面那些结尾比 3 小的子序列，然后把 3 接到最后，就可以形成一个新的递增子序列，而且这个新的子序列长度加一**。

显然，可能形成很多种新的子序列，但是我们只选择最长的那一个，把最长子序列的长度作为 `dp[5]` 的值即可。

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210908112516.gif)

结合定义的base case ,整体的$\huge {C}ode$如下

```java
public int lengthOfLIS(int[] nums) {
    int[] dp = new int[nums.length];
    // base case：dp 数组全都初始化为 1
    Arrays.fill(dp, 1);
    for (int i = 0; i < nums.length; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j]) 
                dp[i] = Math.max(dp[i], dp[j] + 1);
        }
    }
    // 要重新遍历一次数组,找到最长的递增子序列长度
    int res = 0;
    for (int i = 0; i < dp.length; i++) {
        res = Math.max(res, dp[i]);
    }
    return res;
}
```

总结一下如何找到动态规划的状态转移关系：

1、明确 `dp` 数组的定义。这一步对于任何动态规划问题都很重要，如果不得当或者不够清晰，会阻碍之后的步骤。

2、根据 `dp` 数组的定义，运用数学归纳法的思想，假设 `dp[0...i-1]` 都已知，想办法求出 `dp[i]`，一旦这一步完成，整个题目基本就解决了。

但如果无法完成这一步，很可能就是 `dp` 数组的定义不够恰当，需要重新定义 `dp` 数组的含义；或者可能是 `dp` 数组存储的信息还不够，不足以推出下一步的答案，需要把 `dp` 数组扩大成二维数组甚至三维数组。

## 二维递增子序列

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210908161550.png)

这道题目其实是最长递增子序列（Longes Increasing Subsequence，简写为 LIS）的一个变种，因为很显然，每次合法的嵌套是大的套小的，相当于找一个最长递增的子序列，其长度就是最多能嵌套的信封个数。

但是难点在于，标准的 LIS 算法只能在数组中寻找最长子序列，而我们的信封是由 `(w, h)` 这样的二维数对形式表示的。

这道题的解法是比较巧妙的：

**先对宽度 `w` 进行升序排序，如果遇到 `w` 相同的情况，则按照高度 `h` 降序排序。之后把所有的 `h` 作为一个数组，在这个数组上计算 LIS 的长度就是答案。**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210908162523.jpeg)

然后在 `h` 上寻找最长递增子序列：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210908162554.jpeg)

这个子序列就是最优的嵌套方案。

这个解法的关键在于，对于宽度 `w` 相同的数对，要对其高度 `h` 进行降序排序。因为两个宽度相同的信封不能相互包含的，逆序排序保证在 `w` 相同的数对中最多只选取一个。

```java
// envelopes = [[w, h], [w, h]...]
public int maxEnvelopes(int[][] envelopes) {
    int n = envelopes.length;
    // 按宽度升序排列，如果宽度一样，则按高度降序排列
    Arrays.sort(envelopes, new Comparator<int[]>() 
    {
        public int compare(int[] a, int[] b) {
            return a[0] == b[0] ? 
                b[1] - a[1] : a[0] - b[0];
        }
    });
    // 对高度数组寻找 LIS
    int[] height = new int[n];
    for (int i = 0; i < n; i++)
        height[i] = envelopes[i][1];

    return lengthOfLIS(height);
}
/* 返回 nums 中 LIS 的长度 */
public int lengthOfLIS(int[] nums) {
    int piles = 0, n = nums.length;
    int[] top = new int[n];
    for (int i = 0; i < n; i++) {
        // 要处理的扑克牌
        int poker = nums[i];
        int left = 0, right = piles;
        // 二分查找插入位置
        while (left < right) {
            int mid = (left + right) / 2;
            if (top[mid] >= poker)
                right = mid;
            else
                left = mid + 1;
        }
        if (left == piles) piles++;
        // 把这张牌放到牌堆顶
        top[left] = poker;
    }
    // 牌堆数就是 LIS 长度
    return piles;
}
```

再到多维的问题，就涉及到了算法中一个比较难的偏序问题，这也不是业余选手应该考虑的问题了。。。

## 最大子数组

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210908164402.jpeg)

定义 `dp` 数组的含义：

**以 `nums[i]` 为结尾的「最大子数组和」为 `dp[i]`**。

使用数学归纳法来找状态转移关系，`dp[i]` 有两种「选择」，要么与前面的相邻子数组连接，形成一个和更大的子数组；要么不与前面的子数组连接，自成一派，自己作为一个子数组。

```java
int maxSubArray(int[] nums) {
    int n = nums.length;
    if (n == 0) return 0;
    int[] dp = new int[n];
    // base case
    // 第一个元素前面没有子数组
    dp[0] = nums[0];
    // 状态转移方程
    for (int i = 1; i < n; i++) {
        dp[i] = Math.max(nums[i], nums[i] + dp[i - 1]);
    }
    // 得到 nums 的最大子数组
    int res = Integer.MIN_VALUE;
    for (int i = 0; i < n; i++) {
        res = Math.max(res, dp[i]);
    }
    return res;
}
```

**注意到 `dp[i]` 仅仅和 `dp[i-1]` 的状态有关**，那么我们可以进行「状态压缩」，将空间复杂度降低：

```java
int maxSubArray(int[] nums) {
    int n = nums.length;
    if (n == 0) return 0;
    // base case
    int dp_0 = nums[0];
    int dp_1 = 0, res = dp_0;

    for (int i = 1; i < n; i++) {
        // dp[i] = max(nums[i], nums[i] + dp[i-1])
        dp_1 = Math.max(nums[i], nums[i] + dp_0);
        dp_0 = dp_1;
        // 顺便计算最大的结果
        res = Math.max(res, dp_1);
    }
    
    return res;
}
```

## 最长公共子序列

计算最长公共子序列（Longest Common Subsequence，简称 LCS）是一道经典的动态规划题目。

它的解法是典型的二维动态规划，大部分字符串问题都和这个问题是一个思路。

LCS问题就是要我们求两个字符串的LCS长度。

比如说输入 `s1 = "zabcde", s2 = "acez"`，它俩的最长公共子序列是 `lcs = "ace"`，长度为 3，所以算法返回 3。

<hr/>

一个最简单的暴力算法就是，把 `s1` 和 `s2` 的所有子序列都穷举出来，然后看看有没有公共的，然后在所有公共子序列里面再寻找一个长度最大的。

显然，这种思路的复杂度非常高，你要穷举出所有子序列，这个复杂度就是指数级的，肯定不实际。

正确的思路是不要考虑整个字符串，而是细化到 `s1` 和 `s2` 的每个字符。

子序列类型的问题，穷举处所有可能的结果都不容易，而且动态规划算法做的就是穷举+剪枝，只要涉及到子序列的问题，就可以考虑使用动态规划来解决。

<hr/>

对于字符串$s1$和$s2$，一般可以构造一个这样的数组：

$dp[i][j] \Rightarrow 对于s1[0..i-1]和s2[0..j-1]$，他们的LCS长度是$dp[i][j]$。

再接着定义$base\quad case$。

让索引值为$0$的行和列表示空串，$dp[0][..]和dp[..][0]$都应该初始化为0。

再去寻找状态转移方程：

对于$s1$和$s2$中的每个字符，就是根据是否在$lcs$中，如果某个字符在$lcs$中，那么这个字符肯定同时存在于$s1$,$s2$。

<hr/>

$ 
 dp(i,j) =
\begin{cases}
dp(i-1.j-1)+1, & \text{if }\text{s1[i]}={s2[j]} \\
max(dp(i-1,j),dp(i,j-1)), & \text{if }\text{s1[i]} \ne {s2[j]}
\end{cases}$

```python
def longestCommonSubsequence(str1, str2) ->	int:
    def dp(i,j):
        # 空串的base case
        if i == -1 or j == -1:
            return 0
        if(str1[i] == str2[j]):
           	# 这边找到一个lcs中的元素
            return dp(i-1,j-1) + 1
        else:
            # 至少有一个不在lcs中
            # 都试一下，谁能让lcs最长，就听谁的
            return max(dp(i-1,j),dp(i,j-1))
    # 想计算str1[0..end]和str2[0..end]中的lcs长度
    return dp(len(str1) - 1, len(str2) - 1)
```

使用$dp \quad table$优化空间

```cpp
int longestCommonSubsequence(string str1, string str2) {
	int m = str1.size(), n = str2.size();
    // 定义：对于s1[0..i-1]和s2[0..j-1]，他们的lcs长度是dp[i][j]
    vector<vector<int>> dp(m + 1, vector<int>(n + 1,0));
    // base case: dp[0][..]==dp[..][0] = 0，已初始化
    
    for(int i = 1;i <=m; i++) {
        for(int j = 1; j <= n; j++) {
            // 状态转移逻辑
            if(str1[i - 1] == str2[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            }else{
                dp[i][j] = max(dp[i][j - 1], dp[i - 1][j]);
            }
        }
    }
    return dp[m][n];
}
```

## 编辑距离

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210908190909.png)

编辑距离问题就是给我们两个字符串 `s1` 和 `s2`，只能用三种操作，让我们把 `s1` 变成 `s2`，求最少的操作数。

**解决两个字符串的动态规划问题，一般都是用两个指针 `i,j` 分别指向两个字符串的最后，然后一步步往前走，缩小问题的规模**。

设两个字符串分别为 “rad” 和 “apple”，为了把 `s1` 变成 `s2`，算法会这样进行：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210908191433.gif)

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210908191450.jpeg)

根据上面的 GIF，可以发现操作不只有三个，其实还有第四个操作，就是什么都不要做（skip）。比如这个情况：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210908191507.jpeg)

因为这两个字符本来就相同，为了使编辑距离最小，显然不应该对它们有任何操作，直接往前移动 `i,j` 即可。

还有一个很容易处理的情况，就是 `j` 走完 `s2` 时，如果 `i` 还没走完 `s1`，那么只能用删除操作把 `s1` 缩短为 `s2`。比如这个情况：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210908191524.jpeg)

类似的，如果 `i` 走完 `s1` 时 `j` 还没走完了 `s2`，那就只能用插入操作把 `s2` 剩下的字符全部插入 `s1`。

<hr/>

再整理一下之前的思路：

$base \quad case$是$i$走完了$s1$或$j$走完了$s2$，可以直接返回另一个字符串剩下长度。

对于每个字符$s1[i]$和$s2[j]$，可以有$4$种操作：

```python
if s1[i] == s2[j];
	# skip
	i -= 1
	j -= 1
else:
	三选一：
		插入(insert)
		删除(delete)
		替换(replace)
```

如果 `s1[i]！=s2[j]`，就要对三个操作递归了，稍微需要点思考：

```python
dp(i, j - 1) + 1,    # 插入
# 解释：
# 我直接在 s1[i] 插入一个和 s2[j] 一样的字符
# 那么 s2[j] 就被匹配了，前移 j，继续跟 i 对比
# 别忘了操作数加一
```

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210908192654.gif)

```python
dp(i - 1, j) + 1,    # 删除
# 解释：
# 我直接把 s[i] 这个字符删掉
# 前移 i，继续跟 j 对比
# 操作数加一
```

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210908192719.gif)

```python
dp(i - 1, j - 1) + 1 # 替换
# 解释：
# 我直接把 s1[i] 替换成 s2[j]，这样它俩就匹配了
# 同时前移 i，j 继续对比
# 操作数加一
```

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210908192741.gif)

整理一下就是：

```python
def minDistance(s1, s2) -> int:
    # 定义：dp(i, j) 返回 s1[0..i] 和 s2[0..j] 的最小编辑距离
    def dp(i, j):
        # base case
        if i == -1: return j + 1
        if j == -1: return i + 1
        
        if s1[i] == s2[j]:
            return dp(i - 1, j - 1)  # 啥都不做
        else:
            return min(
                dp(i, j - 1) + 1,    # 插入
                dp(i - 1, j) + 1,    # 删除
                dp(i - 1, j - 1) + 1 # 替换
            )
    
    # i，j 初始化指向最后一个索引
    return dp(len(s1) - 1, len(s2) - 1)
```

如果使用$dp \quad table$记录中间信息：

```java
int minDistance(String s1, String s2) {
    int m = s1.length(), n = s2.length();
    int[][] dp = new int[m + 1][n + 1];
    // base case 
    for (int i = 1; i <= m; i++)
        dp[i][0] = i;
    for (int j = 1; j <= n; j++)
        dp[0][j] = j;
    // 自底向上求解
    for (int i = 1; i <= m; i++)
        for (int j = 1; j <= n; j++)
            if (s1.charAt(i-1) == s2.charAt(j-1))
                dp[i][j] = dp[i - 1][j - 1];
            else               
                dp[i][j] = min(
                    dp[i - 1][j] + 1,
                    dp[i][j - 1] + 1,
                    dp[i-1][j-1] + 1
                );
    // 储存着整个 s1 和 s2 的最小编辑距离
    return dp[m][n];
}

int min(int a, int b, int c) {
    return Math.min(a, Math.min(b, c));
}
```

## 最长回文子序列

子序列相比起子串、子数组更加困难一些，因为前者并不要求连续，就算是穷举也会有很多种情况。

并且子序列问题往往涉及两个字符串，如果是穷举所有可能，复杂度将会到达指数级，使用$dp$往往可以降低到$O(n^2)$级别。

对于一维的$dp$，结构通常如下：

```java
int n = array.length;
int[] dp = new int[n];

for(int i = 1; i < n; i++) {
    for(int j = 0; j < i; j++) {
        dp[i] = 最值(dp[i],dp[j] + ...);
    }
}
```

对于二维的$dp$，结构通常如下：

```java
int n = arr.length;
int[][] dp = new int[n][n];

for(int i = 0; i < n; i++) {
    for(int j = 0; j < n; j++) {
        if(arr[i] == arr[j]) {
            dp[i][j] = dp[i][j] + ...;
        }else{
            dp[i][j] = 最值(...);
        }
    }
}
```

在涉及两个字符串、数组是，$dp$数组含义如下：

在子数组$arr1[0..i]$和子数组$arr2[0..j]$中，要求的子序列长度为$dp[i][j]$。

在只涉及一个字符串、数组时，$dp$数组的含义如下：

在子数组$arr[i..j]$中，要求的子序列长度为$dp[i][j]$。

