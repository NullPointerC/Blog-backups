---
title: leetcode-341-扁平化嵌套列表迭代器
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-02 21:34:51
---

[link](https://leetcode-cn.com/problems/flatten-nested-list-iterator/)

<hr/>

**tags: (dfs + 模拟)**

**题目描述:**

<pre>
给你一个嵌套的整数列表 nestedList 。每个元素要么是一个整数，要么是一个列表；该列表的元素也可能是整数或者是其他列表。请你实现一个迭代器将其扁平化，使之能够遍历这个列表中的所有整数。
实现扁平迭代器类 NestedIterator ：
NestedIterator(List<NestedInteger> nestedList) 用嵌套列表 nestedList 初始化迭代器。
int next() 返回嵌套列表的下一个整数。
boolean hasNext() 如果仍然存在待迭代的整数，返回 true ；否则，返回 false 。
你的代码将会用下述伪代码检测：
initialize iterator with nestedList
res = []
while iterator.hasNext()
    append iterator.next() to the end of res
return res
如果 res 与预期的扁平化列表匹配，那么你的代码将会被判为正确。
</pre>

**示例 1：**

<pre>
输入：nestedList = [[1,1],2,[1,1]]
输出：[1,1,2,1,1]
解释：通过重复调用 next 直到 hasNext 返回 false，next 返回的元素的顺序应该是: [1,1,2,1,1]。 
</pre>

**示例 2：**

<pre>
输入：nestedList = [1,[4,[6]]]
输出：[1,4,6]
解释：通过重复调用 next 直到 hasNext 返回 false，next 返回的元素的顺序应该是: [1,4,6]。
</pre>

****

**思路:**

<pre>
首先我们需要知道给我们的是一个接口,那么我们就不用去关注它的实现类到底是怎么样的!!!
所以这个类似于是数的dfs,叶子节点就是数值
</pre>

**代码:**

```java
public class NestedIterator implements Iterator<Integer> {

    private List<Integer> values;
    private Iterator<Integer> orders;

    public NestedIterator(List<NestedInteger> nestedList) {
        values = new LinkedList<>();
        dfs(nestedList);
        orders = values.iterator();
    }

    @Override
    public Integer next() {
        return orders.next();
    }

    @Override
    public boolean hasNext() {
        return orders.hasNext();
    }

    private void dfs(List<NestedInteger> nestedList) {
        for (NestedInteger item : nestedList) {
            if (item.isInteger()) {
                values.add(item.getInteger());
            } else {
                dfs(item.getList());
            }
        }
    }
}
```

![image-20210902214347560](https://gitee.com/cao_ziqiang/img/raw/master/20210902214347.png)

