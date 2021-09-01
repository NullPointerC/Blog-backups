---
title: leetcode-331-验证二叉树的前序序列化
date: 2021-08-31 11:16:21
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/verify-preorder-serialization-of-a-binary-tree/)

<hr/>

**tags:(数论	=>	有向图中入度等于出度)**

**题目描述:**

<pre>
序列化二叉树的一种方法是使用前序遍历。当我们遇到一个非空节点时，我们可以记录
下这个节点的值。如果它是一个空节点，我们可以使用一个标记值记录，例如 #。
     _9_
    /   \
   3     2
  / \   / \
 4   1  #  6
/ \ / \   / \
# # # #   # #
例如，上面的二叉树可以被序列化为字符串 "9,3,4,#,#,1,#,#,2,#,6,#,#"，其
中 # 代表一个空节点。
给定一串以逗号分隔的序列，验证它是否是正确的二叉树的前序序列化。编写一个在不
重构树的条件下的可行算法。
每个以逗号分隔的字符或为一个整数或为一个表示 null 指针的 '#' 。
你可以认为输入格式总是有效的，例如它永远不会包含两个连续的逗号，比如 "1,,3" 。
</pre>

**示例1:**

<pre>
输入: "9,3,4,#,#,1,#,#,2,#,6,#,#"
输出: true
</pre>

**示例2:**

<pre>
输入: "1,#"
输出: false
</pre>

**示例3:**

<pre>
输入: "9,#,#,1"
输出: false
</pre>

**思路:**

<pre>
本题利用了一个有向图中很重要的性质,有向图中入度和出度是相等的;
说实话一开始确实没想到,还傻乎乎去反序列化,构建树,然后比较字符串去了;
看题解才看到这么好的解法,因为树也是一种特殊的有向图,所以也满足上述特性;
根节点提供 2 个出度
根节点以外的真实节点，提供 2 个出度， 1 个入度
null 节点提供 1 个入度
然后将前序序列转数组遍历并且统计入度和出度即可;
这里还可以做一点剪枝,就是在没有遍历完数组时,不会出现入度大于出度的情况;
有入度代表还需要子结点挂载,所以要消耗挂载结点;
而出度代表提供了结点,消耗的结点数大于了提供的结点数,显然是不合法的,可以提前退出;
</pre>

借了一位大佬的图:
![image.png](https://gitee.com/cao_ziqiang/img/raw/master/20210831112136.png)



代码:

```java
class Solution {
    public boolean isValidSerialization(String preorder) {
        if (preorder.equals("#")) {
            return true;
        }
        int inDegrees = 0, outDegrees = 0;
        String[] split = preorder.split(",");
        for (int i = 0; i < split.length; i++) {
            if (i == 0) {
                if (split[i].equals("#")) {
                    return false;
                }
                outDegrees += 2;
                continue;
            }
            if (split[i].equals("#")) {
                inDegrees++;
            } else {
                inDegrees++;
                outDegrees += 2;
            }
            if (i != split.length - 1 && inDegrees >= outDegrees) {
                return false;
            }
        }
        return inDegrees == outDegrees;
    }
}
```

![image-20210831112633536](https://gitee.com/cao_ziqiang/img/raw/master/20210831112633.png)