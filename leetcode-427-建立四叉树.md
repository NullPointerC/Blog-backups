---
title: leetcode-427-建立四叉树
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 54d73d57
date: 2021-09-03 13:13:02
---

[link](https://leetcode-cn.com/problems/construct-quad-tree/)

<hr/>

tags: (dfs+模拟)

**题目描述:**

<pre>
给你一个 n * n 矩阵 grid ，矩阵由若干 0 和 1 组成。请你用四叉树表示该矩阵 
grid 。
你需要返回能表示矩阵的 四叉树 的根结点。
注意，当 isLeaf 为 False 时，你可以把 True 或者 False 赋值给节点，两种
值都会被判题机制 接受 。
四叉树数据结构中，每个内部节点只有四个子节点。此外，每个节点都有两个属性：
val：储存叶子结点所代表的区域的值。1 对应 True，0 对应 False；
isLeaf: 当这个节点是一个叶子结点时为 True，如果它有 4 个子节点则为 False 。
class Node {
    public boolean val;
    public boolean isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;
}
我们可以按以下步骤为二维区域构建四叉树：
如果当前网格的值相同（即，全为 0 或者全为 1），将 isLeaf 设为 True ，将 
val 设为网格相应的值，并将四个子节点都设为 Null 然后停止。
如果当前网格的值不同，将 isLeaf 设为 False， 将 val 设为任意值，然后如下
图所示，将当前网格划分为四个子网格。
使用适当的子网格递归每个子节点。
</pre>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210903131541.png)

<pre>
四叉树格式：
输出为使用层序遍历后四叉树的序列化形式，其中 null 表示路径终止符，其下面不存
在节点。
它与二叉树的序列化非常相似。唯一的区别是节点以列表形式表示 [isLeaf, val] 。
如果 isLeaf 或者 val 的值为 True ，则表示它在列表 [isLeaf, val] 中的值
为 1 ；如果 isLeaf 或者 val 的值为 False ，则表示值为 0 。
</pre>

**示例 1：**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210903131618.png)

<pre>
输入：grid = [[0,1],[1,0]]
输出：[[0,1],[1,0],[1,1],[1,1],[1,0]]
解释：此示例的解释如下：
请注意，在下面四叉树的图示中，0 表示 false，1 表示 True 。
</pre>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210903131639.png)

**示例 2：**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210903131646.png)

<pre>
输入：grid = [[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,1,1,1,1],[1,1,1,1,1,1,1,1],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0]]
输出：[[0,1],[1,1],[0,1],[1,1],[1,0],null,null,null,null,[1,0],[1,0],[1,1],[1,1]]
解释：网格中的所有值都不相同。我们将网格划分为四个子网格。
topLeft，bottomLeft 和 bottomRight 均具有相同的值。
topRight 具有不同的值，因此我们将其再分为 4 个子网格，这样每个子网格都具有相同的值。
解释如下图所示：
</pre>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210903131707.png)

**示例 3：**

<pre>
输入：grid = [[1,1],[1,1]]
输出：[[1,1]]
</pre>

**示例 4：**

<pre>
输入：grid = [[0]]
输出：[[1,0]]
</pre>

**示例 5：**

<pre>
输入：grid = [[1,1,0,0],[1,1,0,0],[0,0,1,1],[0,0,1,1]]
输出：[[0,1],[1,1],[1,0],[1,0],[1,1]]
</pre>

**思路:**

<pre>
这题本来是不难,关键在于读懂题目意思;
首先我们要判断这一个范围内的数组数值是否为同一个数,如果是,则说明,这是一个叶子
节点,然后再根据区间的数的取值来确定val即可;
如果不为同一个数,那么说明这不是一个叶子节点,那么我们需要将这个区域一分为四,
递归的对分好的四个区域构建子树;
</pre>

代码:

```java
class Solution {
    public Node construct(int[][] grid) {
        return build(grid, 0, grid[0].length, 0, grid.length);
    }

    private Node build(int[][] grid, int left, int right, int top, int bottom) {
        Node root = null;
        int key = grid[top][left];
        for (int i = top; i < bottom; i++) {
            for (int j = left; j < right; j++) {
                if (grid[i][j] != key) {
                    Node topLeft = build(grid, left, (left + right) / 2, top, (top + bottom) / 2);
                    Node topRight = build(grid, (left + right) / 2, right, top, (top + bottom) / 2);
                    Node bottomLeft = build(grid, left, (left + right) / 2, (top + bottom) / 2, bottom);
                    Node bottomRight = build(grid, (left + right) / 2, right, (top + bottom) / 2, bottom);
                    root = new Node(false, false, topLeft, topRight, bottomLeft, bottomRight);
                    return root;
                }
            }
        }
        root = new Node(key == 1 ? true : false, true);
        return root;
    }
}
```

![image-20210903132051993](https://gitee.com/cao_ziqiang/img/raw/master/20210903132052.png)

