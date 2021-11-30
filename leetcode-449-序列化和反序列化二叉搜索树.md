---
title: leetcode-449-序列化和反序列化二叉搜索树
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-06 18:59:15
---

[link](https://leetcode-cn.com/problems/serialize-and-deserialize-bst/)

<hr/>

$tags:(dfs/bfs)$

$题目描述$

<pre>
序列化是将数据结构或对象转换为一系列位的过程，以便它可以存储在文件或内存缓冲区中，或通过网络连接链路传输，以便稍后
在同一个或另一个计算机环境中重建。
设计一个算法来序列化和反序列化 二叉搜索树 。 对序列化/反序列化算法的工作方式没有限制。 您只需确保二叉搜索树可以序
列化为字符串，并且可以将该字符串反序列化为最初的二叉搜索树。
编码的字符串应尽可能紧凑。
</pre>


$示例1:$

<pre>
输入：root = [2,1,3]
输出：[2,1,3]
</pre>

$示例2:$

<pre>
输入：root = []
输出：[]
</pre>

思路:

<pre>
如果不考虑题目说到的这是一颗BST的话,那么很简单,用dfs或者bfs都是可以的
</pre>

代码:

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
        TreeNode res = new TreeNode(Integer.parseInt(strs[index]));
        index++;
        res.left = str2Node();
        res.right = str2Node();
        return res;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec ser = new Codec();
// Codec deser = new Codec();
// String tree = ser.serialize(root);
// TreeNode ans = deser.deserialize(tree);
// return ans;
```

如果考虑到这是一颗$BST$，那么我们再考虑到构建一颗树时，需要前序$or$后序的结果再配合中序的结果就可以构建好一颗树，所以在对于$BST$的处理时，只需要在知道前序$or$后序结果即可；

在序列化时，我们要考虑对空节点的处理，序列化的结果就是根节点+“，”+左孩子+“，”+右孩子

比如，对于如下的一颗树

<pre>
   		 2 （root，其中 # 代表空节点 null）
       /  \
      1    3
     / \  / \
    #   ##   4
            / \
           #   #
</pre>

前序遍历： $res$ = $[2,1,null,null,3,null,4,null,null]$

利用$StringBuilder$的字符串拼接，“，”作为分隔符，“#”作为$null$的替代，在调用了序列化后，就得到了前序遍历的结果；

对于反序列化，只需将字符串转列表，每次取列表中第一个元素从列表中删除即可；

代码：

```java
public class Codec {

    String SEP = ",";
    String NULL = "#";
    public String serialize(TreeNode root) {
        
        StringBuilder sb = new StringBuilder();
        serialize(root, sb);
        return sb.toString();
    }

    private void serialize(TreeNode root, StringBuilder sb) {

        if (root == null) {

            sb.append(NULL).append(SEP);
            return ;
        }
        sb.append(root.val).append(SEP);
        serialize(root.left, sb);
        serialize(root.right, sb);
    }

    public TreeNode deserialize(String data) {
        
        LinkedList<String> nodes = new LinkedList<>();
        for (String s : data.split(SEP)) nodes.add(s);

        return deserialize(nodes);
    }

    private TreeNode deserialize(LinkedList<String> nodes) {

        if (nodes.isEmpty()) return null;

        String first = nodes.removeFirst();
        if (first.equals(NULL)) return null;
        TreeNode root = new TreeNode(Integer.parseInt(first));

        root.left = deserialize(nodes);
        root.right = deserialize(nodes);

        return root;
    }
}
```

$bfs$

```java
public class Codec {

    //把树转化为字符串（使用BFS遍历）
    public String serialize(TreeNode root) {
        //边界判断，如果为空就返回一个字符串"#"
        if (root == null)
            return "#";
        //创建一个队列
        Queue<TreeNode> queue = new LinkedList<>();
        StringBuilder res = new StringBuilder();
        //把根节点加入到队列中
        queue.add(root);
        while (!queue.isEmpty()) {
            //节点出队
            TreeNode node = queue.poll();
            //如果节点为空，添加一个字符"#"作为空的节点
            if (node == null) {
                res.append("#,");
                continue;
            }
            //如果节点不为空，把当前节点的值加入到字符串中，
            //注意节点之间都是以逗号","分隔的，在下面把字符
            //串还原二叉树的时候也是以逗号","把字符串进行拆分
            res.append(node.val + ",");
            //左子节点加入到队列中（左子节点有可能为空）
            queue.add(node.left);
            //右子节点加入到队列中（右子节点有可能为空）
            queue.add(node.right);
        }
        return res.toString();
    }

    //把字符串还原为二叉树
    public TreeNode deserialize(String data) {
        //如果是"#"，就表示一个空的节点
        if (data == "#")
            return null;
        Queue<TreeNode> queue = new LinkedList<>();
        //因为上面每个节点之间是以逗号","分隔的，所以这里
        //也要以逗号","来进行拆分
        String[] values = data.split(",");
        //上面使用的是BFS，所以第一个值就是根节点的值，这里创建根节点
        TreeNode root = new TreeNode(Integer.parseInt(values[0]));
        queue.add(root);
        for (int i = 1; i < values.length; i++) {
            //队列中节点出栈
            TreeNode parent = queue.poll();
            //因为在BFS中左右子节点是成对出现的，所以这里挨着的两个值一个是
            //左子节点的值一个是右子节点的值，当前值如果是"#"就表示这个子节点
            //是空的，如果不是"#"就表示不是空的
            if (!"#".equals(values[i])) {
                TreeNode left = new TreeNode(Integer.parseInt(values[i]));
                parent.left = left;
                queue.add(left);
            }
            //上面如果不为空就是左子节点的值，这里是右子节点的值，注意这里有个i++，
            if (!"#".equals(values[++i])) {
                TreeNode right = new TreeNode(Integer.parseInt(values[i]));
                parent.right = right;
                queue.add(right);
            }
        }
        return root;
    }
}
```

