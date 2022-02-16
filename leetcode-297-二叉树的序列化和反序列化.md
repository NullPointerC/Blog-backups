---
title: leetcode-297-二叉树的序列化和反序列化
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: a5c1bbf4
date: 2021-08-30 22:46:20
---

[link](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

<hr/>

tags:(dfs+模拟)

**题目描述:**

<pre>
序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数
据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相
反方式重构得到原数据。
请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化
算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串
反序列化为原始的树结构。
提示: 输入输出格式与 LeetCode 目前使用的方式一致，详情请参阅 LeetCode 序
列化二叉树的格式。你并非必须采取这种方式，你也可以采用其他的方法解决这个问
题。
</pre>

**示例 1：**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210830224748.jpeg)



<pre>
输入：root = [1,2,3,null,null,4,5]
输出：[1,2,3,null,null,4,5]
</pre>

**示例 2：**

<pre>
输入：root = []
输出：[]
</pre>

**示例 3：**

<pre>
输入：root = [1]
输出：[1]
</pre>

**示例 4：**

<pre>
输入：root = [1,2]
输出：[1,2]
</pre>

**思路:**

<pre>
这题的思路不难想到,个人认为难点在于字符串的处理;
首先是dfs或者bfs对树进行搜索,并把搜索的结果用字符串保存起来;
遇到空节点,也直接添加字符串null,如果遇到非空结点,那么将结点的值添加;
反序列化时,先将序列化的结果字符串转列表,每次取第一个元素,并且对其判断是否为字符串null;
如果为null,则直接返回null,否则就返回一个以改值的结点;
</pre>

**代码:**

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {

    private int index = 0;
    private String[] strs;

    public String serialize(TreeNode root) {
        StringBuilder res = new StringBuilder();
        node2Str(root, res);
        return res.toString();
    }

    private void node2Str(TreeNode root, StringBuilder str) {
        if (null == root) {
            str.append("null,");
            return;
        }
        str.append(root.val);
        str.append(",");
        node2Str(root.left, str);
        node2Str(root.right, str);
    }

    public TreeNode deserialize(String data) {
        strs = data.split(",");
        index = 0;
        return str2Node();
    }

    private TreeNode str2Node() {
        if ("null".equals(strs[index])) {
            index++;
            return null;
        }
        TreeNode res = new TreeNode(Integer.valueOf(strs[index]));
        index++;
        res.left = str2Node();
        res.right = str2Node();
        return res;
    }
}


// Your Codec object will be instantiated and called as such:
// Codec ser = new Codec();
// Codec deser = new Codec();
// TreeNode ans = deser.deserialize(ser.serialize(root));
```

![image-20210830225150449](https://gitee.com/cao_ziqiang/img/raw/master/20210830225150.png)

