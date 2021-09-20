---
title: leetcode-133-克隆图
date: 2021-09-17 17:26:13
categories: LeetCode
tags: [algorithm,Java,LeetCode]

---

[link](https://leetcode-cn.com/problems/clone-graph/)

<hr/>

![image-20210917172719365](https://gitee.com/cao_ziqiang/img/raw/master/20210917172719.png)

<hr/>

![image-20210917173157809](https://gitee.com/cao_ziqiang/img/raw/master/20210917173157.png)

又不会写,我是$fw$,摊牌了,看到大佬是用的$dfs$来解决的,先用一个$map$来存$oldNode\quad->\quad newNode$的关系,

然后在对节点深搜,因为这个节点类似于是图的关系。

在遍历途中，每遍历到一个节点就创建一个它的副本并添加进$map$中去，并对当前节点的邻居节点也进行深搜，因为深搜当前节点的返回值将会是当前节点的副本，所以往正在遍历的节点的副本中添加其邻居的副本即可。

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/

class Solution {
    Map<Node,Node> visted = new HashMap<>();
    public Node cloneGraph(Node node) {
        if(node == null) {
            return null;
        }
        return dfs(node);
    }
    
    private Node dfs(Node node) {
        if(!visted.containsKey(node)) {
            // 每遍历一个节点就创建一个它的副本到哈希表
            Node newNode = new Node(node.val);
            visted.put(node, newNode);
            List<Node> nodes = new ArrayList<>();
            // 对它的邻居也进行深搜
            for(Node neighbor : node.neighbors) {
                nodes.add(dfs(neighbor));
            }
            newNode.neighbors = nodes;
        }
        return visted.get(node);
    }
}
```

